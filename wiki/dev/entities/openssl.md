---
title: OpenSSL
created: 2026-04-25
updated: 2026-04-25
type: entity
tags: [security, tooling, reference, learning]
sources: [raw/articles/ios-install-custom-fonts-from-profile.md, raw/articles/ios-app-record-get-certificate-public-key.md]
confidence: high
---

# OpenSSL

## 概览
OpenSSL 在当前 wiki 中主要承担两个角色：其一是签名 `.mobileconfig` 配置描述档，其二是从证书导出链路中提取指纹和公钥。

## 常见用途
- `openssl smime -sign`：签名配置描述档。
- `openssl pkcs12`：从 p12 导出证书。
- `openssl x509`：查看指纹、导出公钥。

## 注意点
- OpenSSL 3 对旧格式材料常需要 `-legacy`。
- 输入与输出格式需要明确区分 PEM、DER 和 PKCS#12。

## 相关页面
- [[ios-app-distribution-certificate-public-key]]
- [[ios-custom-font-installation]]

