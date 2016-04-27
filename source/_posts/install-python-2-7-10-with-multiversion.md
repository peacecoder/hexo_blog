---
title: install_python_2.7.10_with_multiversion
date: 2016-04-19 20:39:53
tags: centos, python, pip
---
if your centos has not install openss-devel, please install it

```shell
yum install openssl-devel
```

## Download Python 2.7.10 Source Code

```shell
cd /opt
wget --no-check-certificate https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tar.xz
sudo wget --no-check-certificate https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tar.xz
sudo tar xf Python-2.7.10.tar.xz 
```


## Compile Souce Code

```shell
cd Python-2.7.10
sudo ./configure --prefix=/usr/local
sudo make && sudo make altinstall
```

*Note: Please pay attendtion to last cmd, we should use altinstall intead of install, or the python2.6 will be replaced by python2.7*


## Install pip2.7

```shell
sudo wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
sudo /usr/local/bin/python2.7 ez_setup.py
sudo /usr/local/bin/easy_install-2.7 pip
```

## Install Python Module

```shell
sudo /usr/local/bin/pip2.7 install pykafka
```
