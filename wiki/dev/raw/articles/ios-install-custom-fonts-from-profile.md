---
source_url: file:///Users/yin.yan/Documents/yanyin1986.github.io/开发笔记/iOS安装自定义字体.md
ingested: 2026-04-25
sha256: fc1135e4e477bc1c3fefcb025bfacb74affc25dfb1aa698fa734d1076c2df0e4
---


## 背后原理概览

- Fonts payload（com.apple.font）：苹果自带支持的 iOS 配置描述档类型，可将单个 .ttf 或 .otf 字体打包到 .mobileconfig 中，由用户手动安装后系统就可调用 。 
- VPN 与证书管理页面：iOS 将所有用户手动安装的配置描述档都展示在 “VPN 与设备管理 / 证书信任” 页面中，即使描述档中没有 VPN 或证书 payload 也一样出现，因此很多人误以为是通过 VPN 安装字体 。
- 证书签名问题：若描述档未签名（或签发方尚未信任），安装后还需在此页面「信任根证书」，才允许 Fonts payload 生效。 
 

## 实现方式举例

### 方法一：使用 macOS 上的 Apple Configurator 2（官方 GUI 工具）
1. 在 Mac App Store 安装 Apple Configurator 2。 
2. 打开后创建新配置档（⌘N）。 
3. 在 “General” 中输入名称、标识符等。 
4. 添加 Fonts 模块，选择 .ttf 或 .otf 字体（不支持 .ttc/.otc）。
   - 每个 Fonts payload 中仅能包含一个字体文件，多字体需多个 payload。 
5.（可选）可添加 Certificate payload，用来打入你的 CA 来签名，避免“未受信任”的提示。
6. 导出 .mobileconfig 文件，可选择 UNSIGNED（实验）或 DER‑签名格式。
7. 将文件上传至 HTTPS 链接或邮件附件，通过 Safari 下载（必须由 Safari 发起）。 
8. 安装时依次点击：允许 → \[已下载的描述档/注册“XXX”\] → 安装 → 输入解锁密码。 
9. 如含根证书 payload，则必须手动启用「完全信任」才被允许安装字体。 
 
安装后，可在 Notability、Pages、Word、Textastic 等支持自定义字体的 app 中使用。

📝 注意：
- 字体最大建议小于 ~20 MB，否则 iOS 安装容易失败；
- 描述档有效期约 2 年，过期后需卸载重新导入；
- 无法更改 iOS 默认系统字体，如锁屏、控制中心、Spotlight 等界面保持原样。  

### 方法二：使用 iPhone 上的开源应用（如 Fontcase，AnyFont，iFont 等） 

这类 App 使用的原理与上面基本一致，但简化了配置生成过程： 
1. 在 App 中导入字体（.ttf/.otf）；
2. App 自动生成配置档（.mobileconfig）并弹出本地 HTTP 页面，点击“安装”；
3. Safari 下载后，你会看到 “已下载的描述档 XXX” 出现在 VPN 与设备管理页面；
4. 点进去“安装”即可完成字体安装。
> 有 Reddit 用户描述：「在 App 导入字体后，点击 install 会下载配置档，然后进入 Settings > VPN & Device Management 就能看到这个字体 profile，点进去安装」 。 
5. 这些 App 通常还会内嵌一个小型证书 payload 或签名，保障配置档能顺利安装。 


### 方法三：自己搭建脚本服务器自动生成签名 .mobileconfig
如果你希望更灵活地生成字体包，可按以下流程脚本化： 
1. 将 .ttf 或 .otf 以 Base64 嵌入到 XML 模板的 <key>Font</key><data>…</data>；
2. <key>PayloadType</key><string>com.apple.font</string> 必填； 
3. 可选择加一个 com.apple.security.root 的 Certificate payload； 
4. 然后用 openssl smime -sign 签名，输出 DER 格式的 .mobileconfig； 
5. 放到你的 HTTPS 服务器，通过 Safari 访问。 
6. 安装过程与 Apple Configurator 一样。 

📌 样例字段模板（简略）： 
```xml
<dict>
  <key>PayloadType</key><string>Configuration</string>
  <key>PayloadIdentifier</key><string>com.example.myfonts</string>
  <key>PayloadVersion</key><integer>1</integer>
  <key>PayloadDisplayName</key><string>自定义字体</string>
  <key>PayloadContent</key>
  <array>
    <dict>
      <key>PayloadType</key><string>com.apple.font</string>
      <key>Font</key><data>…base64 字体内容…</data>
      <key>Name</key><string>CustomFont.ttf</string>
    </dict>
    <!-- 可选 插入根证书 payload 放这里 -->
  </array>
</dict>
```

然后用：
```shell
openssl smime -sign \
  -in unsigned.mobileconfig \
  -out myfont.mobileconfig \
  -signer your.crt \
  -inkey your.key \
  -certfile ca-chain.crt \
  -outform der -nodetach 
```
