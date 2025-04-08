+++
date = '2025-04-08T22:08:22+09:00'
draft = false
title = 'GPT-SoVITS V3 Setup'
+++

# 安装步骤

## 1. 安装 conda

```bash
curl -O https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
bash ~/Anaconda3-2024.10-1-Linux-x86_64.sh
```

安装完了之后，会提示 restart shell，如果是 linux 服务器，则可以 exit 然后重新连接。

## 2. 创建 conda 环境

```bash
conda create -n GPTSoVits python=3.9
conda activate GPTSoVits
```

## 3. 克隆 GPT-SOVITS 项目并且进行初步安装

```bash
git clone https://github.com/RVC-Boss/GPT-SoVITS.git
cd GPT-SoVITS
bash install.sh
```

## 4. 查漏补缺

```bash
pip install -r requirements.txt
```

## 5. 安装 git-lfs

```bash
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get update
sudo apt-get install git-lfs
```

## 6. 下载 pretrained 模型

按照官方的文档介绍，需要将 pretrained 模型下载到 `{project_root}/GPT_SoVITS/pretrained_models` 下面。
但是，`{project_root}/GPT_SoVITS/pretrained_models` 已经存在在 git 的 index 里面了，所以这边用了笨办法。

```bash
git lfs install
git clone https://huggingface.co/lj1995/GPT-SoVITS models
mv models/* GPT-SoVITS/GPT_SoVITS/pretrained_models/
rm -rf models
```
