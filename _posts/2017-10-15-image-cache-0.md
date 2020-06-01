---
title: 图片以及缓存（0）：总纲
date: 2017-10-15 07:01:00 Z
categories:
- CoreAnimation
- CoreGraphics
- ImageIO
- UIKit
- iOS
- 图像
layout: post
---

## 0x00 概述
这篇是总纲（没有像九阴真经用梵文来写 😅 ）。

工作了很多年，在这些基数实现的细节方面，一直都只是浅尝辄止，没有深入挖掘，所以痛定思痛，就开了这个系列好好得去研究每个小细节。

现在流行的App，特别是社交类，电商类，图片类产品，需要展示大量的图片，那么我们就需要用到一些图片处理，缓存管理的库来处理。最近一直在研究市面上常用的几个用来做这个功能的库。


## 0x01 图片缓存库
### Objective-C
- [SDWebImage](https://github.com/rs/SDWebImage) 估计是最常用的缓存库了
- [FastImageCache](https://github.com/path/FastImageCache) 对图片展示的极致优化，特别是小图片
- [YYImage](https://github.com/ibireme/YYImage) 处理各种图片格式，底层用了很多C的库去处理
- [AFNetworking](https://github.com/AFNetworking/AFNetworking) 图片下载展示处理得也很独到
- [FlyImage](https://github.com/northwind/FlyImage) 集合了几家的有点开发的新的缓存库
- [EGOCache](https://github.com/enormego/EGOCache) 缓存库
- [PINCache](https://github.com/pinterest/PINCache) 缓存库
- [Haneke](https://github.com/Haneke/Haneke)

### Swift
- [Kingfisher](https://github.com/onevcat/Kingfisher) 估计是适用范围最广的Swift图片缓存库
- [Nuke](https://github.com/kean/Nuke) 作者做了一个benchmark，显示这个库是所有的库中效率最好的
- [HanekeSwift](https://github.com/Haneke/HanekeSwift)
- [KFSwiftImageLoader](https://github.com/kiavashfaisali/KFSwiftImageLoader) 没有自己实现DiskCache，而是用了苹果提供的URLCache缓存系统来自动缓存文件

### 不再维护
- [TMCache](https://github.com/tumblr/TMCache)

## 0x02 Benchmark
之前有人对以上常用的库做了一个对比打分，可以在[这边](https://github.com/bpoplauschi/ImageCachingBenchmark) 找到相关的测试结果。
但是基于的版本有一些老，并且没有比较这几个用Swift写的库，之后有时间，会对新的库做一些新的benchmark。

## 0x03 打算写几个部分
- [x] CoreGraphics图片渲染以及字节对齐 #4 
- [ ] CALayer的属性设置，效率问题 #5 
- [ ] 文件读取与mmap的比较与应用场景 #6 
- [ ] iOS7之后第三方应用不能硬件解码jpeg #7 
- [ ] 从NSCache，URLCache说各种缓存

----
## 引用
1. [iOS图片加载速度极限优化—FastImageCache解析](https://blog.cnbang.net/tech/2578/)
2. [YYCache 设计思路](https://blog.ibireme.com/2015/10/26/yycache/)
3. [iOS 保持界面流畅的技巧](https://blog.ibireme.com/2015/11/12/smooth_user_interfaces_for_ios/)
4. [Advanced Imaging on iOS](https://www.slideshare.net/rsebbe/2014-cocoaheads-advimaging)