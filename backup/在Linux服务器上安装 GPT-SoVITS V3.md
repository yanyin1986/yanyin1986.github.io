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

---

# 启动

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

# [Optional] Linux 自动启动 webui 和 jupyterlab

因为这个环境是放在服务器上，所以需要设置自动启动。
可以用 systemd 来设置。

> [!IMPORTANT]
> 因为整个环境是构筑在 conda 之上的，所以需要在使用 conda 启用特定环境的情况下，才能启动 webui。

## webui

### 创建 systemd 启动脚本

```bash
vim on_start.sh

#!/bin/bash
source /home/ubuntu/anaconda3/etc/profile.d/conda.sh
conda activate GPTSoVits
python -u /home/ubuntu/GPT-SoVITS/webui.py
```

### 添加可执行权限

```bash
sudo chmod +x on_start.sh
```

### 创建 systemd 的服务文件

```bash
sudo vim /etc/systemd/system/webui.service
```

> [!IMPORTANT]
> systemd 的配置文件必须包含 Unit 的配置，但是因为 code block 插件问题，这边没有写。
> 需要自己补上 Unit 的部分。
> 例如:
>
> [Unit]
>
> Description = GPT-SoVITS webui

```ini
[Service]
Type=simple
ExecStart=/home/ubuntu/GPT-SoVITS/on_start.sh
User=ubuntu
Group=adm
WorkingDirectory=/home/ubuntu/GPT-SoVITS
Restart=always

[Install]
WantedBy = multi-user.target
```

### 启动服务

```bash
sudo systemctl daemon-reload   # 重新加载配置文件
sudo systemctl enable webui   # 开机自启
sudo systemctl start webui
```

## jupyter

### 创建 systemd 启动脚本

```bash
sudo vim /etc/systemd/system/jupyterlab.service
```

> [!IMPORTANT]
> 需要补充 Unit 的部分。
>
> [Unit]
>
> Description = JupyterLab

```bash
[Service]
Type=simple
ExecStart=/home/ubuntu/anaconda3/envs/GPTSoVits/bin/jupyter lab --port 8888
User=ubuntu
Group=adm
WorkingDirectory=/home/ubuntu/GPT-SoVITS
Restart=always

[Install]
WantedBy=multi-user.target
```

### 启动服务

```bash
sudo systemctl daemon-reload   # 重新加载配置文件
sudo systemctl enable jupyterlab   # 开机自启
sudo systemctl start jupyterlab
```
