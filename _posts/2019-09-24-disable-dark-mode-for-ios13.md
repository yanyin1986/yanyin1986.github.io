---
layout: post
title: Disable dark mode for iOS13
date: 2019-09-24 16:22 +0900
---
# 概要
最近在开发的时候遇到一个问题，组里没有设计来对应暗黑模式，然而用**iOS13的SDK**来Build的话，最后就会展现不完美的暗黑模式。
大多数UIView的默认的背景颜色都变成了黑色，整个界面看起来很糟心，还不如不支持暗黑模式来得好。
所以就想着，要不就暂时将系统的UI样式指定为light好了。

Google大法了一把，发现了解决方案。
https://stackoverflow.com/questions/56546267/ios-13-disable-dark-mode-changes

## 局部页面指定light模式

iOS13开始，所有的UIViewController里面多了一个叫`overrideUserInterfaceStyle`的属性，只要在viewDidLoad的时候指定一下即可。

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // overrideUserInterfaceStyle is available with iOS 13
    if #available(iOS 13.0, *) {
        // 只采用light样式
        overrideUserInterfaceStyle = .light
    }
}
```

## 全部页面指定light模式

在App的Info.plist里面添加一对属性即可。
https://developer.apple.com/documentation/bundleresources/information_property_list/uiuserinterfacestyle

```xml
<key>UIUserInterfaceStyle</key>
<string>Light</string>
```
