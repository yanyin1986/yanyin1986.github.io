---
title: easyjapanese 文档向量导入与 few-shots 加载
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [backend, api, workflow, reference]
sources: [raw/articles/easyjapanese-service-gpt-document-index.md]
confidence: medium
---

# easyjapanese 文档向量导入与 few-shots 加载

## 概览
这条笔记总结了 easyjapanese-service-gpt 中两类角色语料的分工：`*_context.txt` 进入 Qdrant 作为语义检索上下文，`*_few_shots.txt` 留在 apiserver 镜像内作为说话风格样本。两者既服务同一条 prompt 链路，又有完全不同的部署与故障模式。

## 数据分工
- `*_context.txt`：由离线的 `document-index` 镜像导入 Qdrant collection。
- `*_few_shots.txt`：由主镜像 CI 打包进 `/tools/document/`，apiserver 通过 `os.Open` 读取，并做 4 小时缓存。
- few-shots 不进入 Qdrant，也不需要额外挂载。

## 导入流程
1. 准备可访问的 Qdrant 实例。
2. 创建 3 个 collection，并保持 1536 / Cosine 配置一致。
3. 准备带有 embedding 与 qdrant.address 的 config.json。
4. 使用 `easyjapanese-document-index` 镜像逐个文件导入。
5. 通过 `points_count` 校验导入是否成功。

## 脆弱点与故障模式
- Qdrant fd 上限不足会导致导入中途失败。
- `WORKDIR` 或 `/tools/` 拷贝路径变化会让 few-shots 读取失败，而且错误可能被静默吞掉。
- collection 维度错误、容器网络不可达、file_path 配错，都会造成典型导入失败。
- 重复执行导入是追加行为，可能产生重复 chunk。

## 为什么值得单独记录
这份文档不只是部署说明，还明确揭示了“检索上下文”和“风格样本”在架构上的分层，以及它们分别依赖的容器镜像、配置项、路径约定和缓存行为。这些信息对后续排障、重构 Dockerfile、迁移 Qdrant 或替换 embedding 模型都很关键。

## 相关页面
- [[easyjapanese-service-gpt]]
- [[qdrant]]

