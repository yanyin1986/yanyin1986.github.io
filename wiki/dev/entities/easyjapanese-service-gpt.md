---
title: easyjapanese-service-gpt
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [backend, api, tooling, project]
sources: [raw/articles/easyjapanese-service-gpt-document-index.md]
confidence: medium
---

# easyjapanese-service-gpt

## 概览
easyjapanese-service-gpt 是一个面向日语学习/对话能力的后端 API 服务。当前导入的文档重点不是完整业务功能，而是它如何把角色语料分为“向量检索上下文”和“few-shots 风格样本”两套链路，并分别交给 Qdrant 与 apiserver 处理。

## 当前关注点
- `*_context.txt` 被离线导入 Qdrant collection，用于运行时语义检索。
- `*_few_shots.txt` 不进入向量库，而是随 apiserver 镜像打包，并在请求时作为风格样本加载。
- 文档明确指出 few-shots 的文件路径依赖 Dockerfile 中 `/tools/document` 的复制与 `WORKDIR /`。

## 运行链路
- 离线导入侧依赖 `document-index` 镜像。
- 在线服务侧依赖 apiserver、Qdrant 与 embedding 配置协同工作。
- prompt 组装链路为 `context + few_shots + history + query`。

## 相关页面
- [[easyjapanese-document-indexing-and-few-shots]]
- [[qdrant]]

