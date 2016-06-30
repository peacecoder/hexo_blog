---
title: logrotate 用法
date: 2016-04-27 22:29:05
tags: linux
---

在linux 系统管理过程当中，我们经常需要对系统或者服务生成的日志做切分处理（P.S. 我们总不希望日志无限期增长，导致磁盘不足 )。我们当然可以对每个日志写不同的日志切分脚本。可是随着服务的增多，这类相似的需求也越来越多。重复造轮的工作简直就是浪费生命，我们倡导站在巨人的肩膀快速的实现既定的目标。

前辈们已经早就很好的解决此类需求，这就是logrotate 命令。

其实logrotate命令就是一个定期的crontab 脚本而已。在centos下，该命令如下所示

```shell
#!/bin/sh

/usr/sbin/logrotate /etc/logrotate.conf >/dev/null 2>&1
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0
```
运行的时候会调用`/etc/logrotate.conf`配置文件，具体信息如下所示

```
# see "man logrotate" for details
# rotate log files weekly
weekly

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# uncomment this if you want your log files compressed
#compress

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d

# no packages own wtmp -- we'll rotate them here
/var/log/wtmp {
    monthly
    minsize 1M
    create 0664 root utmp
    rotate 1
}

# system-specific logs may be also be configured here.
```

根据上述的文件内容我们知道，我们只需要配置好`配置文件`并将该文件放置`/etc/logrotate.d`目录中，那么每天都会定时的调用logrotate对相应的日志按照配置信息进行切分。
这就是logrotate的原理。

## xianxi

## 参考

* [logroate](http://www.linuxcommand.org/man_pages/logrotate8.html)
