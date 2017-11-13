---
title: redis
date: 2017-11-13 22:09:42
tags: redis, lua
---

1. 如何获取redis中lua的版本信息
写一个lua脚本, 如下所示
```
return _VERSION
```

然后执行 `redis-cli --eval $script.lua`
