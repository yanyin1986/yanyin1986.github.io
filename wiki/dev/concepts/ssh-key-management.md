---
title: SSH 密钥管理
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [security, tooling, workflow, reference]
sources: [raw/articles/multiple-git-accounts-on-one-device.md]
confidence: medium
---

# SSH 密钥管理

## 概览
SSH 密钥管理是多账号 Git 协作的基础。关键不是只生成密钥，而是把密钥命名、agent 注入、Host 别名和 remote 规范串成一个稳定流程。

## 实践要点
- 每个身份使用独立密钥文件。
- 在 `~/.ssh/config` 中使用可读的 Host 别名。
- 将私钥加入 agent 后再做连接测试。
- 仓库 remote 地址必须与 Host 别名保持一致。

## 相关页面
- [[git]]
- [[multiple-git-accounts-on-one-device]]

