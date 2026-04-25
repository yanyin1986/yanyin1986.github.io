---
title: GPT-SoVITS
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [tooling, learning, reference, workflow]
sources: [raw/articles/linux-install-gpt-sovits-v3.md]
confidence: medium
---

# GPT-SoVITS

## 概览
GPT-SoVITS 是一个语音相关项目。在当前 wiki 中，这个实体页主要记录其在 Linux 服务器上的部署关注点：环境隔离、模型下载、Web UI 启动，以及长期运行的服务化。

## 当前关注点
- 使用 conda 维持隔离环境。
- 预训练模型需要额外拉取并放入指定目录。
- 服务器部署通常需要结合 systemd 做长期运行。

## 相关页面
- [[linux-server-gpt-sovits-v3-setup]]
- [[systemd-service-setup]]

