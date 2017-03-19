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
