---
title: 同一台设备上配置多个 Git 账号
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [tooling, workflow, reference, personal]
sources: [raw/articles/multiple-git-accounts-on-one-device.md]
confidence: medium
---

# 同一台设备上配置多个 Git 账号

## 概览
这条笔记记录了如何在一台机器上为不同 Git 账号维护独立 SSH 身份。核心思路不是切换全局配置，而是为每个身份生成独立密钥，并在 `~/.ssh/config` 中用不同 Host 别名映射到同一个 GitHub 主机。

## 操作步骤
1. 为额外账号生成独立 SSH 密钥。
2. 将公钥上传到对应账号的 GitHub SSH keys。
3. 在 `~/.ssh/config` 中添加新的 Host 配置，指定独立 `IdentityFile`。
4. 通过 `ssh-add` 将私钥加入 agent。
5. clone 仓库时使用别名主机，例如 `git@demo:org/repo.git`。

## 关键配置
- 默认账号可以继续绑定 `github.com`。
- 新账号通过类似 `Host demo` 的方式进行别名映射。
- 仓库 remote 地址必须改成别名主机，否则仍会命中默认密钥。

## 适用场景
- 个人账号与公司账号并存。
- 多组织协作且每个组织使用不同凭据。
- 需要避免频繁改动全局 Git/SSH 配置。

## 相关页面
- [[git]]
- [[ssh-key-management]]

