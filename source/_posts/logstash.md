---
title: logstash
date: 2017-03-21 09:44:58
tags: log, es, elk
---

logstash file rotate and start_position

1. logstash使用inode来确定一个文件是否是新文件
1. logstash 中start_position 如果表示beginning 表示从新文件的第一行开始处理，如果不是新文件的话，那么就从sincedb中查看对应inode的offset，从offset的位置开始处理
1. log4j使用mv（rename并非copy）方式rotate日志文件，因此rotate 生成的日志文件将不会被logstash 再次处理
