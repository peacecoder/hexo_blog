---
title: install_m2crypto
date: 2017-03-19 15:21:22
tags: python, pip, library
---

I have spend a lot of time to install m2crypo in mac. here is some problems I come accoss in this process.

```shell
# pip install m2crypto --user python
```

## Cannot find swig
```
brew install swig
```
solve this problem, but new error follows.

## Cannot find header file

```
Collecting m2crypto
  Using cached M2Crypto-0.25.1.tar.gz
Requirement already satisfied: typing in /Users/bin.yu/Library/Python/2.7/lib/python/site-packages (from m2crypto)
Installing collected packages: m2crypto
  Running setup.py install for m2crypto ... error
    Complete output from command /usr/bin/python -u -c "import setuptools, tokenize;__file__='/private/tmp/pip-build-4LjWjW/m2crypto/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-Fvea5n-record/install-record.txt --single-version-externally-managed --compile:
    running install
    running build
    running build_py
    copying M2Crypto/__init__.py -> build/lib.macosx-10.11-intel-2.7/M2Crypto
    copying M2Crypto/_m2crypto.py -> build/lib.macosx-10.11-intel-2.7/M2Crypto
    ...
    copying M2Crypto/SSL/SSLServer.py -> build/lib.macosx-10.11-intel-2.7/M2Crypto/SSL
    copying M2Crypto/SSL/timeout.py -> build/lib.macosx-10.11-intel-2.7/M2Crypto/SSL
    copying M2Crypto/SSL/TwistedProtocolWrapper.py -> build/lib.macosx-10.11-intel-2.7/M2Crypto/SSL
    running build_ext
    building 'M2Crypto.__m2crypto' extension
    swigging SWIG/_m2crypto.i to SWIG/_m2crypto_wrap.c
    swig -python -I/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7 -I/usr/include -includeall -modern -builtin -outdir build/lib.macosx-10.11-intel-2.7/M2Crypto -o SWIG/_m2crypto_wrap.c SWIG/_m2crypto.i
    SWIG/_m2crypto.i:31: Error: Unable to find 'openssl/opensslv.h'
    SWIG/_m2crypto.i:45: Error: Unable to find 'openssl/safestack.h'
    SWIG/_evp.i:12: Error: Unable to find 'openssl/opensslconf.h'
    SWIG/_ec.i:7: Error: Unable to find 'openssl/opensslconf.h'
    error: command 'swig' failed with exit status 1
    
    ----------------------------------------
Command "/usr/bin/python -u -c "import setuptools, tokenize;__file__='/private/tmp/pip-build-4LjWjW/m2crypto/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-Fvea5n-record/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /private/tmp/pip-build-4LjWjW/m2crypto/
```

The error is obvious, it is because it cannot find the openssl include directory. However, I have installed openssl by brew before and swig includes `-I/usr/include`, but my openssl's header directory is `/usr/local/opt/openssl/include`, so one option is create a soft link between them. Here is the command.

```
# sudo ln -s /usr/local/opt/openssl/include/openssl /usr/include/openssl
ln: /usr/include/openssl: Operation not permitted
```

this error is because new version of Mac OS EL Capitan 10.11.6 is not allow to operate some critical path, you can refer to this [link](http://apple.stackexchange.com/questions/196224/unix-ln-s-command-not-permitted-in-osx-el-capitan-beta3). It is not recommended to close rootless. So we have to find a better way to solve this problem.

another option is check swig command

```
swig -python -I/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7 -I/usr/include -includeall -modern -builtin -outdir build/lib.macosx-10.11-intel-2.7/M2Crypto -o SWIG/        _m2crypto_wrap.c SWIG/_m2crypto.i
```

by `swig --hel` we find that

```
Options can also be defined using the SWIG_FEATURES environment variable, for example:

  $ SWIG_FEATURES="-Wall"
  $ export SWIG_FEATURES
  $ swig -python interface.i

is equivalent to: 

  $ swig -Wall -python interface.i 
```
so we can set env variable `SWIG_FEATURES` to change header path.

```
env LDFLAGS="-L"(brew --prefix openssl)"/lib" \
CFLAGS="-I"(brew --prefix openssl)"/include" \
SWIG_FEATURES="-cpperraswarn -includeall -I"(brew --prefix openssl)"/include" \
pip install m2crypto
```

for more detail please refer to [Trouble installing m2crypto with pip on El Capitan](http://stackoverflow.com/questions/33005354/trouble-installing-m2crypto-with-pip-on-el-capitan).

