---
title: pem2bks
date: 2017-09-18 18:13:42
tags: ssl, keytool, openssl
---
## Download bcprod-ext-jdk15on-158
1. download the jar from https://www.bouncycastle.org/latest_releases.html
2. put the jar to `/usr/libexec/java_home`/jre/lib/ext

## Convert ca pems to bks


```
keytool -importcert -trustcacerts -storetype BKS -provider org.bouncycastle.jce.provider.BouncyCastleProvider -alias $alias -keystore $bks_file -file $pem_file
```

## Convert cert&key to bks

```
cd $1
cn=$1
rm -f ${cn}.p12 ${cn}.bks
openssl pkcs12 -export -in ${cn}.crt -inkey ${cn}.key -out ${cn}.p12
keytool -v -importkeystore -srckeystore ${cn}.p12 -srcstoretype PKCS12 -destkeystore ${cn}.bks -deststoretype BKS -destalias ${cn} -srcalias 1 -–provider org.bouncycastle.jce.provider.BouncyCastleProvider
rm -f ${cn}.p12
```

error:
Problem importing entry for alias 1: java.security.KeyStoreException: java.io.IOException: Error initialising store of key store: java.security.InvalidKeyException: Illegal key size.

download jce from http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html

unzip the file and replace the file in `/usr/libexec/java_home`/jre/lib/security


Reference:
1. http://callistaenterprise.se/blogg/teknik/2011/11/24/creating-self-signed-certificates-for-use-on-android/
2. http://blog.majiajie.me/2016/05/11/Android-%E5%81%B6%E9%81%87HTTPS/
3. https://www.pixelstech.net/article/1468485248-Convert-JKS-to-BKS-using-keytool
