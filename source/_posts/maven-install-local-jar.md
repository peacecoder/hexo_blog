---
title: maven 安装本地JAR包
draft: true
date: 2016-04-27 20:32:59
tags: java, maven
---

在使用maven 管理Java 项目中，我们经常会遇到需要将本地Jar包依赖的需求。本文简单介绍一下如何通过maven导入本地jar包

## 操作

### 安装本地Jar包

在项目中添加libs/目录并将依赖的jar放到该目录中
``` xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-install-plugin</artifactId>
    <version>2.5.2</version>
    <executions>
        <execution>
            <id>install-local-jar</id>
            <phase>initialize</phase>
            <goals>
                <goal>install-file</goal>
            </goals>
            <configuration>
                <groupId>com.company.proj</groupId>
                <artifactId>proj_code</artifactId>
                <version>0.0.4</version>
                <packaging>jar</packaging>
                <file>${basedir}/libs/proj_jar_name.jar</file>
            </configuration>
        </execution>
    </executions>
</plugin>
```
同时在dependencies中添加依赖，如下所示

``` xml
<dependency>
   <groupId>com.xiaomi.miliao</groupId>
   <artifactId>xmpush-server-api</artifactId>
   <version>0.0.4-SNAPSHOT</version>
</dependency>
```

### 安装

``` shell
mvn initialize
mvn clean install
```

### 解释

原理也是很简单的，通过maven-install-plugin 这个插件，利用`mvn initialize`命令， 可以将本地的jar包安装到本地的repo中~/.m2/repository/...
此后，由于项目依赖该jar包，会在本地的repo中查找，找到了就能直接打包进去了。

是不是很简单？当然还有其他方式能够达到同样的目的，例如直接利用文件路径，但是个人不推荐这样的方式。感兴趣的童鞋可以自己google对比一下吧

## 参考
