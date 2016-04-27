---
title: Linux 字符串替换合集
date: 2016-03-29 20:46:03
categories: Linux
tags: [Linux, awk, python, sed, grep, tr]
description: 该文主要描述如何使用Linux 命令行实现字符串以及文本替换
---

sed

``` shell
ILLEGAL_CHARS=$(python -c 'print u"\u3000".encode("utf8")')
sed 's/['"${ILLEGAL_CHARS}"']//g' $file_name
```

