---
title: Qdrant
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [database, backend, tooling, reference]
sources: [raw/articles/easyjapanese-service-gpt-document-index.md]
confidence: medium
---

# Qdrant

## 概览
Qdrant 在这份文档里承担 easyjapanese-service-gpt 的向量检索存储层。`*_context.txt` 被切分并转成 1536 维向量后写入不同 collection，供 apiserver 按 query 做语义检索。

## 当前配置点
- collection 向量参数为 1536 维，距离度量为 Cosine。
- embedding 模型记录为 `text-embedding-3-small`。
- 导入工具运行时要求 `qdrant.address` 从容器视角可达。

## 运维注意点
- Qdrant 容器需要较高 `ulimits.nofile`，否则可能出现 `Too many open files`。
- 重复导入默认是追加，不是覆盖；需要先删 collection 再重建。
- 如果 collection 维度不匹配，会触发 `Vector dimension error`。

## 相关页面
- [[easyjapanese-service-gpt]]
- [[easyjapanese-document-indexing-and-few-shots]]

