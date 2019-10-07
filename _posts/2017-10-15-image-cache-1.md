---
title: 图片以及缓存（1）：图片渲染以及字节对齐
date: 2017-10-15 08:01:00 Z
categories:
- CoreAnimation
- CoreGraphics
- ImageIO
- UIKit
- iOS
- 图像
layout: post
---

## 0x00 界面渲染


## 0x02 字节对齐

在path开源的FastImageCache里，
[How Fast Image Cache Works](https://github.com/path/FastImageCache#how-fast-image-cache-works) 的章节里面看到了关于字节对齐的描述，非常想了解，所以就开始搜索了一下网络上关于这个细节的描述。在[iOS图片加载速度极限优化—FastImageCache解析](https://blog.cnbang.net/tech/2578/) 里面也看到了关于这个细节的描述，但是作者只是简单的提到了这么一段话：

> 那什么是字节对齐呢，按我的理解，为了性能，底层渲染图像时不是一个像素一个像素渲染，而是一块一块渲染，数据是一块块地取，就可能遇到这一块连续的内存数据里结尾的数据不是图像的内容，是内存里其他的数据，可能越界读取导致一些奇怪的东西混入，所以在渲染之前CoreAnimation要把数据拷贝一份进行处理，确保每一块都是图像数据，对于不足一块的数据置空。大致图示：(pixel是图像像素数据，data是内存里其他数据)
> 块的大小应该是跟CPU cache line有关，ARMv7是32byte，A9是64byte，在A9下CoreAnimation应该是按64byte作为一块数据去读取和渲染，让图像数据对齐64byte就可以避免CoreAnimation再拷贝一份数据进行修补。FastImageCache做的字节对齐就是这个事情。

写了几个小demo去研究这里面的玄机。

----
## 引用
1. [How Fast Image Cache Works](https://github.com/path/FastImageCache#how-fast-image-cache-works)
2. [iOS图片加载速度极限优化—FastImageCache解析](https://blog.cnbang.net/tech/2578/) 
3. [iOS image caching. Libraries benchmark (SDWebImage vs FastImageCache)](https://bpoplauschi.wordpress.com/2014/03/21/ios-image-caching-sdwebimage-vs-fastimage/)
4. [使用SDWebImage和YYImage下载高分辨率图，导致内存暴增的解决办法](http://www.jianshu.com/p/1c9de8dea3ea)
5. [SDWebImage源码阅读系列](http://www.cnblogs.com/polobymulberry/category/785704.html)