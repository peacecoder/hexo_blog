---
title: convert_pem_to_jks
date: 2017-03-29 14:04:39
tags:
---

# Convert PrivateKey/Certificate to JKS


openssl pkcs12 -export -in nmp_client.crt -inkey nmp_client.key -out nmp_client.pfx
nmp_client.crt nmp_client.key


keytool -genkey -keyalg RSA -alias endeca -keystore nmp_client_truststore.ks

keytool -delete -alias endeca -keystore nmp_client_truststore.ks


../ca/nmp_ca.crt

keytool -import -v -trustcacerts -alias endeca-ca -file ../ca/nmp_ca.crt -keystore nmp_client_truststore.ks


keytool -genkey -keyalg RSA -alias endeca -keystore nmp_client_keystore.ks
keytool -delete -alias endeca -keystore nmp_client_keystore.ks


keytool -v -importkeystore -srckeystore nmp_client.pfx -srcstoretype PKCS12 -destkeystore nmp_client_keystore.ks -deststoretype JKS


## Reference
https://docs.oracle.com/cd/E35976_01/server.740/es_admin/src/tadm_ssl_convert_pem_to_jks.html
https://blogs.technet.microsoft.com/uclobby/2015/05/22/merge-certificate-public-and-private-key-with-openssl/
