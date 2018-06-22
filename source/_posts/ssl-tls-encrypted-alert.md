---
title: ssl_tls_encrypted_alert
date: 2018-06-21 19:15:42
tags: ssl, tls, encrypted alert
---
# Background
We establish mqtt connection between client and broker, and client will pulish a lot of messages. Howerver our client developer find a very strange problem that if the publish load increase, sometimes the client will lost connection, or even worse crash!!!

Then we try to dig deeper and find the root cause from both client and broker view. We use [mosquitto client](https://github.com/eclipse/mosquitto). And we use netty to build mqtt broker.

## Client's Problems
We use tcpdump to find if any abnormal tcp packet exists.

```shell
tcpdump -i eth0 tcp port 20083 -w ./eth0_$(date +"%Y%m%d_%H%M%S").pcap
```

![ws_client_error_1](content/images/2018/06/ws_client_error_1.jpeg)

From the blue retangle we can find that, after client send Encrypted Alert, an FINACK as follow.
Recently, our client develop we found a very strange SSLException: Tag unmatch!

```json
{
    "@timestamp": "2018-06-21T13:57:12.023+08:00",
    "@version": 1,
    "level": "ERROR",
    "level_value": 40000,
    "message": "Other error: javax.net.ssl.SSLException: Tag mismatch!",
    "thread_name": "nioEventLoopGroup-3-2"
}
```

## Reference
1. [一个最简单的破解SSL加密网络数据包的方法](https://blog.csdn.net/zhubaitian/article/details/43816167)
2. [SSL/TLS的bad record MAC问题排查](https://blog.csdn.net/gufachongyang02/article/details/52154124)
3. [wireshark解密用临时秘钥加密的ssl/tls数据包](https://blog.csdn.net/gufachongyang02/article/details/52166285)
4. [Alert Protocol](https://en.wikipedia.org/wiki/Transport_Layer_Security#Alert_protocol)
