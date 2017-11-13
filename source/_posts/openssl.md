---
title: openssl
date: 2017-03-05 22:54:20
tags:
---

# Useful command

```shell
# Extract chain
openssl crl2pkcs7 -nocrl -certfile tls_tsp_test_p12.pem | openssl pkcs7 -print_certs -out tls_tsp_test_trustchain.pem

# Extract key
openssl pkey -in tls_tsp_test_p12.pem -out ~/Nextev/message_server/conf/test_tsp/ssl/nmp_server.key

# Extract cert
openssl x509 -in tls_tsp_test_p12.pem -outform pem -out ~/Nextev/message_server/conf/test_tsp/ssl/  nmp_server.crt

# Verify client by connect to server
openssl s_client -host localhost -port 10083 -certform PEM -cert tls_lion_cert.pem -keyform PEM -key tls_lion_privKey.pem -CAfile nmp_server_chain.crt
```

https://www.sslshopper.com/article-most-common-openssl-commands.html


```
Android 系统中使用的证书要求是bks格式。
一般来说，我们使用jdk的keytool只能生成jks的证书库，如果生成bks的则需要下载BouncyCastle库。
搜集了各方资料，整理了以下如何将服务端提供的crt格式证书转换成Android上使用的bks证书。

1 Introduction

JKS和JCEKS是Java密钥库(KeyStore)的两种比较常见类型，JKS的Provider是SUN，在每个版本的JDK中都有。
BKS来自BouncyCastleProvider，它使用的也是TripleDES来保护密钥库中的Key，它能够防止证书库被不小心修改（Keystore的keyentry改掉1个bit都会产生错误），BKS能够跟JKS互操作。
2 Steps

要生成bks证书，需要bcprov-ext-jdk15on-151.jar（下载地址）。且将该文件放到Java\jdk1.8.0_20\jre\lib\ext目录下。

我们的后端同事提供了自签名的服务器证书server.crt，我们需要把这个server.crt转换成Android系统的bks格式证书。使用以下命令行：
keytool -importcert -trustcacerts -keystore e:\key.bks -file e:\server.crt -storetype BKS -provider org.bouncycastle.jce.provider.BouncyCastleProvider
按照提示重复输入两次密码（在Java的KeyStore对象加载证书时会用到这个密码。），然后就成功将E:\目录下的server.crt转成key.bks证书。
```
