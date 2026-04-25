---
title: iOS 安装自定义字体
created: 2026-04-25
updated: 2026-04-25
type: concept
tags: [tooling, workflow, reference, personal]
sources: [raw/articles/ios-install-custom-fonts-from-profile.md]
confidence: medium
---

# iOS 安装自定义字体

## 概览
这条笔记整理了在 iOS 上安装自定义字体的几种可行路径，核心机制是通过配置描述档中的 `com.apple.font` payload 将 `.ttf` 或 `.otf` 字体安装到系统可调用范围内。它适合做 iOS 工具链补充知识，而不是应用内字体打包方案。

## 核心原理
- iOS 通过配置描述档承载字体 payload，因此安装入口会出现在“VPN 与设备管理/证书管理”相关页面。
- 若配置描述档未签名，或者签发链未被信任，则可能需要额外信任证书后字体才会生效。
- 这种方式只能为支持自定义字体的 App 提供字体，不能替换系统 UI 字体。

## 实现路径
### Apple Configurator 2
- 在 macOS 上新建配置描述档并添加 Fonts 模块。
- 一个 Fonts payload 只能承载一个字体文件，多字体需要多个 payload。
- 可选加入证书 payload，并导出为签名后的 `.mobileconfig`。

### 第三方 App
- 类似 Fontcase、AnyFont、iFont 这类应用，本质上也是生成并安装 `.mobileconfig`。
- 优点是流程简单，适合移动端直接操作。

### 脚本化生成
- 可将字体以 Base64 形式嵌入 XML 模板。
- 使用 `openssl smime -sign` 生成 DER 格式的签名 `.mobileconfig`。
- 适合需要自动化批量分发字体的场景。

## 实操注意点
- 字体建议控制在约 20MB 以下，否则安装成功率会下降。
- 配置描述档存在有效期，过期后通常需要重新导入。
- 字体需要通过 Safari 发起安装流程，普通文件传输路径并不总是可用。

## 相关页面
- [[ios-app-distribution-certificate-public-key]]
- [[openssl]]

