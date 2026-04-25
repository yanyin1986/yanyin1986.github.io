---
title: systemd 服务配置
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [devops, workflow, reference, learning]
sources: [raw/articles/linux-install-gpt-sovits-v3.md]
confidence: medium
---

# systemd 服务配置

## 概览
systemd 适合把长期运行的开发服务包装为可重启、可开机自启的后台进程。当前导入笔记中，它主要用于 GPT-SoVITS Web UI 和 JupyterLab 的守护。

## 实践要点
- service 文件应包含完整的 `[Unit]`、`[Service]` 和 `[Install]` 区块。
- 对于 conda 环境，需要在启动脚本中显式初始化并激活环境。
- `WorkingDirectory`、`User` 与 `Restart` 策略是常见关键字段。

## 相关页面
- [[gpt-sovits]]
- [[linux-server-gpt-sovits-v3-setup]]

