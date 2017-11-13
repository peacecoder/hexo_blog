---
title: logback
date: 2017-08-16 20:25:42
tags:
---

集成logback 的时候碰到一个问题，就是先有很多第三方的lib使用的都是log4j。因此需要保证与之兼容。

## 2.4 与遗留Logging框架兼容
目前行业除了Logback之外，广泛使用的还有其他四种Logging框架：

1. Log4j 1 (http://logging.apache.org/log4j/1.2/)     
1. Log4j 2 (http://logging.apache.org/log4j/2.x/)     
1. java.util.logging (http://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html)     
1. Apache commons Logging (http://commons.apache.org/proper/commons-logging/)     

Log4j2 因为是刚出来的，目前SLF4J对其的兼容性还未知，对于其他的三种框架，SLF4J都提供了兼容性的支持。下面介绍了如何让Logbak兼容这些框架，另外，也可以阅读官方说明：http://www.slf4j.org/legacy.html


#### 2.4.1 兼容Log4j 1和Apache Commons Logging

SLF4J对于Log4J 1和Apache commons Logging的支持方式是提供了实现Log4j和Apache commons Logging接口的SLF4J实现。使用方式是

1. 去取对Log4J和Apache commons Logging的Jar包的引用     
2. 引入SLF4J的对应接口的实现包。     
 

#### 2.4.1.1 移除引用

如果你的系统是直接的使用了Log4j或者Apache commons Logging框架的话，你可以直接把对他们的引用去掉就可以了。如果是你所引用的第三方包里面引用了Log4j或者Apache commons Logging，可以使用<exclusions>标签去掉对他们的引用，如下所示：

```
<dependency>
  <groupId>org.springframework.ldap</groupId>
  <artifactId>spring-ldap-core</artifactId>
  <exclusions>
      <exclusion>
          <artifactId>commons-logging</artifactId>
          <groupId>commons-logging</groupId>
      </exclusion>
  </exclusions>
</dependency>
``` 

如何找到哪些第三方包引用了Log4j或者Apache commons Logging呢？有俩个方法：
1. 使用 mvn dependency:tree 命令，如下图所示，可以看出需要在org.springframework.ldap:spring-ldap-core中排除掉对Apache commons Logging的引用。

2. 第二种方式是使用Eclipse的m2e Maven插件。如下图所示，打开pom.xml文件后，选择Dependency Hierarchy标签，然后在Filter中输入logging或者log4j进行过滤，在左侧的Dependency Hierarchy中使用右键菜单就可以自动过滤了。 

#### 2.4.1.2 Maven导入对应的SLF4J实现包

```
<!-- Log4j 的SLF4J 实现 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>log4j-over-slf4j</artifactId>
    <version>1.7.6</version>
</dependency>
<!-- Apache commons Logging 的SLF4J实现 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>jcl-over-slf4j</artifactId>
    <version>1.7.6</version>
</dependency>
```

#### 2.4.1.3 非Maven版导入对应的SLF4J实现包

直接删除掉log4j-1.**.jar和commons-logging-1.**.jar文件，把http://slf4j.org/dist/slf4j-1.7.6.zip下载下来，把压缩包里的log4j-over-slf4j-1.7.6.jar或者（和）jcl-over-slf4j-1.7.6.jar文件放到classpath中。

 

### 2.4.2 兼容java.util.logging

SLF4J的jul-to-slf4j模块实现了一个java.util.logging handler，该handler会把对java.util.logging的调用都转化成对SLF4J实现的调用。所以需要以下俩个步骤：

1. 导入jul-to-slf4j模块     
1. 启用jul-to-slf4j模块     
 

#### 2.4.2.1 导入jul-to-slf4j模块maven版

 
```
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>jul-to-slf4j</artifactId>
    <version>1.7.6</version>
</dependency>
```

#### 2.4.2.2 导入jul-to-slf4j模块非Maven版

把http://slf4j.org/dist/slf4j-1.7.6.zip下载下来，把压缩包里的jul-to-slf4j-1.7.6.jar放到classpath中。

#### 2.4.2.3 启用jul-to-slf4j模块

在logging.properties中添加如下一行：

```
handlers = org.slf4j.bridge.SLF4JBridgeHandler 
```

# Reference
http://www.cnblogs.com/justuntil/p/4067160.html
