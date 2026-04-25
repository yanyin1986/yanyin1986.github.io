---
title: iOS App 备案获取证书公钥
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [security, workflow, reference, personal]
sources: [raw/articles/ios-app-record-get-certificate-public-key.md]
confidence: medium
---

# iOS App 备案获取证书公钥

## 概览
这条笔记整理了为 iOS App 备案或证书校验场景提取证书公钥的步骤，包括从 keychain 导出 `.p12`、通过 OpenSSL 提取证书、查看指纹以及导出公钥。

## 操作流程
1. 创建新的 distribution 证书。
2. 从本地 keychain 导出为 `Certificates.p12`。
3. 使用 `openssl pkcs12` 提取公钥证书。
4. 使用 `openssl x509` 查看 MD5 或 SHA1 指纹。
5. 导出 PEM 格式公钥供备案或其他校验流程使用。

## 版本兼容性
- 在 OpenSSL 3 中需要增加 `-legacy` 参数，避免读取旧格式 p12 时失败。
- 指纹和公钥提取都基于 `.crt` 文件继续执行。

## 适用场景
- iOS App 备案材料准备。
- 证书指纹核对。
- 需要单独提交或审查公钥的流程。

## 相关页面
- [[openssl]]
- [[ios-custom-font-installation]]

