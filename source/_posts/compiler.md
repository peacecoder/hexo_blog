---
title: compiler
date: 2017-12-06 22:19:34
tags: g++, c++, compiler
---

G++ in mac cannot find ssl.h

and then setting the CPPFLAGS and LDFLAGS to point to the openssl lib from brew
```
export CPPFLAGS=-I/usr/local/opt/openssl/include
export LDFLAGS=-L/usr/local/opt/openssl/lib
```
