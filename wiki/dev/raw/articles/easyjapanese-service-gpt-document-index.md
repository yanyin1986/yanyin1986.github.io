---
source_url: file:///Users/yin.yan/Documents/mmd/easyjapanese-service-gpt/docs/document-index.md
ingested: 2026-04-25
sha256: 5390fbc38b40a864b9e74d936cc2ebeb7cec2a873dc2429f96ac59b90d48d987
---

# 文档向量库导入与 few-shots 加载

服务用到两类「角色语料」，分工不同，部署方式也不同。

## 数据分工

| 数据 | 存放位置 | 谁负责 | 运行时如何读取 |
|---|---|---|---|
| `*_context.txt` | Qdrant 向量库 | 离线用 `document-index` 镜像灌入 | apiserver 按 query 语义检索 |
| `*_few_shots.txt` | apiserver 镜像内 `/tools/document/` | 主镜像 CI 构建时自动打包 | apiserver 启动后 `os.Open` 读文件 + 4h 内存缓存 |

`few_shots` 文件**不进 Qdrant**，也**不需要**挂载 —— 主镜像里已经有了。

## Collection 与文件对应表

| 源文件 | Qdrant Collection | 主服务硬编码位置 |
|---|---|---|
| `blue.txt` | `blue_collection` | `internal/repository/persistence/mysql/chatbot.go:429` |
| `sanma_context.txt` | `sanma_collection` | `chatbot.go:476` |
| `naruto_context.txt` | `naruto_collection` | `chatbot.go:516` |

向量参数：**1536 维 / Cosine**（`internal/infrastructure/qdrant/collection.go:14-17`）。
Embedding 模型：`text-embedding-3-small`（按 config 中 `llm.embedding`）。

---

## 导入步骤

### 1. 准备 Qdrant

```bash
# Qdrant 容器（已有 docker-compose）
docker ps | grep qdrant

# 验证可达
curl http://127.0.0.1:6333/collections
```

> ⚠️ **必须给 Qdrant 容器配 `ulimits.nofile`**，否则导入到一半会报
> `Too many open files`。docker-compose 示例：
>
> ```yaml
> services:
>   qdrant:
>     image: qdrant/qdrant:v1.1.1
>     ulimits:
>       nofile:
>         soft: 65536
>         hard: 65536
> ```
>
> 验证生效：`docker exec qdrant sh -c 'ulimit -n'` 应输出 `65536`。

### 2. 建好 3 个 collection

```bash
for name in blue_collection sanma_collection naruto_collection; do
  curl -X PUT http://127.0.0.1:6333/collections/$name \
    -H 'Content-Type: application/json' \
    -d '{"vectors":{"size":1536,"distance":"Cosine"}}'
  echo
done
```

### 3. 拉取 document-index 镜像

```bash
docker pull registry.cn-hongkong.aliyuncs.com/mushare/easyjapanese-document-index:1.3.2
```

### 4. 准备 config.json

工具运行时需要一份 config.json，至少包含：
- `llm.embedding`（用于生成向量）
- `qdrant.address`（grpc 地址，**容器视角**能解析到）

> 注意 `qdrant.address`：
> - 如果本机就是 qdrant 宿主机 → 写 `127.0.0.1:6334`，容器用 `--network host`
> - 如果走 VPC 内网 IP → 确保从 docker 宿主机能 `nc -vz <IP> 6334`

### 5. 灌数据（一个文件一条命令）

```bash
CONFIG=/path/to/config.json
IMAGE=registry.cn-hongkong.aliyuncs.com/mushare/easyjapanese-document-index:1.3.2

run_one () {
  local col=$1 file=$2
  echo "=== importing $file -> $col ==="
  docker run --rm --network host \
    -v "$CONFIG:/config.json:ro" \
    -e collection_name="$col" \
    -e file_path="$file" \
    "$IMAGE"
}

run_one blue_collection   /tools/document/blue.txt
run_one sanma_collection  /tools/document/sanma_context.txt
run_one naruto_collection /tools/document/naruto_context.txt
```

每个文件：每 chunk 调一次 OpenAI embedding + sleep 2s。`blue.txt` 约 971 chunks → **~30 分钟**。

后台跑：

```bash
docker run -d --name doc-blue --network host \
  -v "$CONFIG:/config.json:ro" \
  -e collection_name=blue_collection \
  -e file_path=/tools/document/blue.txt \
  "$IMAGE"
docker logs -f doc-blue
```

### 6. 验证

```bash
for n in blue_collection sanma_collection naruto_collection; do
  echo -n "$n: "
  curl -s http://127.0.0.1:6333/collections/$n | jq '.result.points_count'
done
```

`points_count` 不为 0 即成功。

---

## few-shots 加载链路（仅说明，无操作）

apiserver 每次接到 chat 请求：

1. 用 query 在 Qdrant `*_collection` 检索 → 拿到 context 文档片段
2. 从 `/tools/document/*_few_shots.txt` 随机抽**连续 10 行**作为说话风格样本（`atom.go:172`）
3. 拼成 prompt：`context + few_shots + history + query` → Azure GPT-4o-mini

文件内容首次读取后缓存 4 小时（`atom.go:196`），不会反复读盘。

### 关键脆弱点

代码使用相对路径 `./tools/document/sanma_few_shots.txt`，依赖主 Dockerfile 这两行才能对上：

```dockerfile
COPY --from=build /app/tools/ /tools
WORKDIR /
```

任何人改了 `WORKDIR` 或 `tools/` 拷贝路径 → `os.Open` 失败 → `readFewShotsFile` 静默返回 `nil`（`atom.go:186-188` 吞了错误）→ bot 角色感丢失但不会报错。

> 修主镜像时记得验证：
> `docker run --rm --entrypoint ls <apiserver-image> /tools/document`
> 应能看到 5 个 txt。

---

## 故障排查

| 现象 | 原因 |
|---|---|
| 容器秒退 + `open ./config.json: no such file` | `-v` 挂载路径错 |
| `collection_name is empty` | 没传 `-e collection_name=...` |
| `open /tools/document/xxx: no such file` | `file_path` 写错（镜像里仅 5 个 txt） |
| grpc dial timeout | `qdrant.address` 在容器视角不可达 — 检查 `--network` |
| `Wrong input: Vector dimension error` | collection 不是 1536 维（删了重建） |
| 重复跑同一文件 | 数据**追加**，不是覆盖。如需重灌：`curl -X DELETE .../collections/<name>` 后重建 |
| Qdrant 报 `RocksDB ... Too many open files` | 容器 fd 上限不够，按"准备 Qdrant"小节配 `ulimits.nofile: 65536` 后重启容器；中断的 collection 建议删了重灌避免 chunk 重复 |
