+++
date = '2025-04-08T22:08:22+09:00'
draft = false
title = '在Linux服务器上安装 GPT-SoVITS V3'
+++

# 安装步骤

## 安装 conda

```bash
curl -O https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
bash ~/Anaconda3-2024.10-1-Linux-x86_64.sh
```

安装完了之后，会提示 restart shell，如果是 linux 服务器，则可以 exit 然后重新连接。

## 创建 conda 环境

```bash
conda create -n GPTSoVits python=3.9
conda activate GPTSoVits
```

## 克隆 GPT-SOVITS 项目并且进行初步安装

```bash
git clone https://github.com/RVC-Boss/GPT-SoVITS.git
cd GPT-SoVITS
bash install.sh
```

## 查漏补缺

```bash
pip install -r requirements.txt
```

## 安装 git-lfs

```bash
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get update
sudo apt-get install git-lfs
```

## 下载 pretrained 模型

按照官方的文档介绍，需要将 pretrained 模型下载到 `{project_root}/GPT_SoVITS/pretrained_models` 下面。
但是，`{project_root}/GPT_SoVITS/pretrained_models` 已经存在在 git 的 index 里面了，所以这边用了笨办法。

```bash
git lfs install
git clone https://huggingface.co/lj1995/GPT-SoVITS models
mv models/* GPT-SoVITS/GPT_SoVITS/pretrained_models/
rm -rf models
```

## 启动 webui

```bash
python webui.py
```

第一次启动的时候会自动下载 g2pW 相关的模型配置，下载完成之后会自动启动 webui。

## [Optional] 安装 jupyterlab

为了操作更加简便，安装 jupyterlab。

```bash
pip install jupyterlab==3.7.6
```

然后生成配置文件。

```bash
jupyter notebook --generate-config
```

然后修改一些配置文件让其可以远程访问。

1. 找到 `c.NotebookApp.allow_remote_access` 并且将其值改为 `True`。
2. 找到 `c.NotebookApp.ip` 并将其值改为 `0.0.0.0`

然后通过下面的命令就可以启动 jupyterlab 了。

```bash
jupyter lab --port=8888
```

## [Optional] Linux 自动启动 webui 和 jupyterlab

因为这个环境是放在服务器上，所以需要设置自动启动。
可以用 systemd 来设置。

```bash

```
