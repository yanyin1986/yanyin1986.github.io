---
title: 关于Universal links
date: 2019-03-29 01:55:00 Z
layout: post
---

# Universal Links

## /AASA 和 /.well-known/AASA
苹果官方文档里面提到需要在服务端安装AASA文件。
不过可以装在两个地方：
```
{DOMAIN}/apple-app-site-association
{DOMAIN}/.well-known/apple-app-site-association
```
自从iOS9.3开始，苹果会优先访问/.well-known/apple-app-site-association，如果访问失败才会访问根目录下的AASA文件。
下面的StackOverFlow里提到了这个问题。👇
[https://stackoverflow.com/questions/34812135/requests-to-well-known-apple-app-site-association/55180870#55180870](https://stackoverflow.com/questions/34812135/requests-to-well-known-apple-app-site-association/55180870#55180870)

---
# 参考引用
1. [https://developer.apple.com/library/archive/qa/qa1916/_index.html](https://developer.apple.com/library/archive/qa/qa1916/_index.html)
