---
title: Linux 服务器安装 GPT-SoVITS V3
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [tooling, workflow, devops, learning]
sources: [raw/articles/linux-install-gpt-sovits-v3.md]
confidence: medium
---

# Linux 服务器安装 GPT-SoVITS V3

## 概览
这条笔记覆盖了在 Linux 服务器部署 GPT-SoVITS V3 的完整流程：安装 conda、创建独立环境、拉取代码、补齐依赖、通过 git-lfs 获取预训练模型，并使用 systemd 配置自启动。

## 基础安装流程
1. 安装 Anaconda。
2. 创建 `GPTSoVits` conda 环境并激活。
3. 克隆 `GPT-SoVITS` 仓库并执行安装脚本。
4. 通过 `pip install -r requirements.txt` 做依赖补齐。
5. 安装 `git-lfs`，再从 Hugging Face 拉取模型并放入 `GPT_SoVITS/pretrained_models/`。

## 启动与补充工具
- Web UI 可通过 `python webui.py` 启动。
- 首次启动会自动下载额外模型。
- 可选安装 JupyterLab，以便在服务器上更方便地管理环境。

## 自动启动
- 笔记强调了 conda 环境激活是 systemd 启动脚本的一部分。
- Web UI 与 JupyterLab 都可以用 systemd service 做守护。
- service 文件至少应补上 `[Unit]` 区块，否则配置不完整。

## 注意点
- 模型目录已经被仓库索引占位，因此下载后采用移动文件的方式填充。
- 启动脚本中要显式 `source` conda 初始化脚本。
- JupyterLab 的远程访问需要修改配置允许远端连接。

## 相关页面
- [[gpt-sovits]]
- [[systemd-service-setup]]

