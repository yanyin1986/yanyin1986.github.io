---
title: 从一个表中查数据，插入到另一个表
date: 2020-11-02 05:39:00 Z
tags:
- SQL
layout: post
---

最近需要将一个表中的数据批量导入到另外一个表，甚至还要指定导入的字段。

有一种通用的SQL语法，可以方便的达到这个目的，MySQL和SQLServer之类的都支持。

## 字段相同
如果`来源表`和`目标表`的字段完全一样，并且希望插入所有的数据，则可以用：
```SQL
INSERT INTO {目标表} SELECT * FROM {来源表}
```

## 字段不同
如果`来源表`和`目标表`的字段不一样，需要指定，则可以用：
```SQL
INSERT INTO {目标表} (目标表.字段1, 目标表.字段2, ...)
SELECT 来源表.字段1, 来源表.字段2, ... FROM {来源表}
```

## 加上其他的条件
当然WHERE之类的条件也是可以的，最后我的SQL为
```SQL
INSERT INTO 
  mail_jobs.mail_jobs(`email`, `status`, `error`) 
  SELECT
    email, 0, ''
  FROM 
    xoxo_im.v2_user
  WHERE xoxo_im.v2_user.id > 48295;
```
