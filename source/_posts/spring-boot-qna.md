---
title: spring_boot_qna
date: 2018-06-05 18:07:08
tags:spring-boot
---
1. IDEA 提示 Spring Boot Annotion processor not found in classpath 解决办法

solution: 

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

reference:
 * https://blog.csdn.net/HeatDeath/article/details/79839829
 * https://blog.csdn.net/lom9357bye/article/details/77458797
 * https://blog.csdn.net/u011659172/article/details/51274345
 * http://www.baeldung.com/properties-with-spring
 * https://www.cnblogs.com/sprinng/p/5622600.html
