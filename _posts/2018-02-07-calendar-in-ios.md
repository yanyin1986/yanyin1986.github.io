---
title: Cocoa的时间框架之 日历类Calendar
date: 2018-02-07 07:01:00 Z
categories:
- Foundation
- iOS
layout: post
---

# 现实生活中的日历
## 1. 概念
历法是用年，月，日等时间单位计算时间的方法。
主要有阳历，阴历和阴阳历三种。

1. 阳历即太阳历，其历年为一个回归年，现时国际通用的公历（西历）即为太阳历的一种，亦简称为阳历
2. 阴历亦称月亮历，或称太阴历，其历月是一个朔望月，历年为12个朔望月，其大月30天，小月29天，伊斯兰历即为阴历的一种
3. 阴阳历的平均历年为一个回归年，历月为朔望月，因为12个朔望月与回归年相差太大，所以阴阳历中设置闰月，所以这种历法与月相相符，也与地球绕太阳周期运动相符合。中国的农历就是阴阳历的一种。

上文部分引用自[维基百科-历法](https://zh.wikipedia.org/wiki/%E5%8E%86%E6%B3%95)。

## 2. 种类
世界上的历法是很多的，在不同的国家地区，会采用不同的历法。绝大多数国家都采用公历，也称格里历，当然也有很多其他的历法沿袭了下来。

- 世界上绝大多数国家采用的[公历](https://zh.wikipedia.org/wiki/%E5%85%AC%E5%8E%86)
- 在南亚和东南亚的佛教国家，比如柬埔寨，泰国等会采用的[佛历](https://zh.wikipedia.org/wiki/%E4%BD%9B%E6%9B%86)
- 在中国广泛采用的[农历](https://zh.wikipedia.org/wiki/%E8%BE%B2%E6%9B%86)
- 以色列国目前采用的[希伯来历](https://zh.wikipedia.org/wiki/%E5%B8%8C%E4%BC%AF%E4%BE%86%E6%9B%86)
- 伊斯兰教国家普遍采用的[伊斯兰历](https://zh.wikipedia.org/wiki/%E4%BC%8A%E6%96%AF%E5%85%B0%E5%8E%86)
- [伊斯兰民事历](https://en.wikipedia.org/wiki/Civil_calendar)
- 日本国采用的[日本历](https://zh.wikipedia.org/wiki/%E6%97%A5%E6%9C%AC%E6%9B%86)
- [民国纪年](https://zh.wikipedia.org/wiki/%E6%B0%91%E5%9C%8B%E7%B4%80%E5%B9%B4)
- 目前在伊朗和阿富汗使用的阳历之[伊朗历](https://zh.wikipedia.org/wiki/%E4%BC%8A%E6%9C%97%E6%9B%86)
- [印度历](https://en.wikipedia.org/wiki/Hindu_calendar)
- [国际标准ISO 8601 日历日期表示法](https://zh.wikipedia.org/wiki/ISO_8601)
- [科普特历](https://zh.wikipedia.org/wiki/%E7%A7%91%E6%99%AE%E7%89%B9%E6%9B%86)

上述列举的只是Cocoa里面明确给了标识符的明确的各种历法，除此之外，还有很多很多种类的历法，这边就不一一赘述了。

## 3. 作用
时间是有序的，但是需要一个标准去定位以及比较，总结。
这就需要历法。
历法是用来帮助人类确定每一天在无限的时间里确切的位置，并记录历史。
又由于历法是具有周期性的，他同时也能够提醒人类一些周期性的变化。
比如，中国的农历，将整个一年划分为24个节气，对农事给予了指导。

# Cocoa中的日历类
## 1. 初始化
我们根据提供的Calendar的标识符去初始化日历类实例。

```swift
init(identifier: Calendar.Identifier)
```
可以看到需要一个Calendar.Identifier，这是一个enum
```swift
case buddhist  // 佛历
case chinese  // 农历
case coptic  // 科普特历
case ethiopicAmeteAlem  // TODO:
case ethiopicAmeteMihret // TODO:
case gregorian  // 公历，格里历
case hebrew  // 希伯来历
case indian  // 印度历
case islamic  // 伊斯兰历
case islamicCivil  // 伊斯兰民事历
case islamicTabular  // TODO:
case islamicUmmAlQura  // TODO:
case iso8601  // ISO8601国际标准日历
case japanese  // 日本历
case persian  // 伊朗历
case republicOfChina  // 民国纪事
```

## 自动获取当前日历
cocoa提供了两种获取当前日历的默认方法
```swift
static var autoupdatingCurrent: Calendar
static var current: Calendar
```
这两者有着一定的区别。


## 闰秒
闰秒是在协调世界时（UTC）中增加或减少一秒，使它与平太阳时贴近所做调整。需要闰秒的部分原因是因为平均太阳日（mean solar day）的长度正以非常缓慢的速度增加中，另一个原因是原子钟赋予秒固定的时间长度。而当结合时，就已经比当时的太阳时的秒短少了一点点[3]。时间现在是以稳定的原子钟来测量（TAI或国际原子时），而地球自转有着许多的变数。


---
## 引用
1. [GNUstep/libs-base/NSCalendar](https://github.com/gnustep/libs-base/blob/a8c2c4965dc57d2edc98003d4cc8ed65e251e39b/Source/NSCalendar.m)
2. [NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)
3. [How to convert NSDate in to relative format as “Today”,“Yesterday”,“a week ago”,“a month ago”,“a year ago”?](https://stackoverflow.com/questions/20487465/how-to-convert-nsdate-in-to-relative-format-as-today-yesterday-a-week-ago)
4. [闰秒](https://zh.wikipedia.org/wiki/%E9%97%B0%E7%A7%92)
5. [iOS时间那点事--NSDate](https://my.oschina.net/yongbin45/blog/150114)
6. [iOS时间那点事--NSDateFormatter](https://my.oschina.net/yongbin45/blog/150667)
7. [iOS时间那点事--NSTimeZone](https://my.oschina.net/yongbin45/blog/151376)
8. [iOS时间那点事--NSLocale](https://my.oschina.net/yongbin45/blog/156130)
9. [iOS时间那点事--NSCalendar NSDateComponents](https://my.oschina.net/yongbin45/blog/156181)
