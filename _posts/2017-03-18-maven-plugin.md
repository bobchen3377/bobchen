---
layout: post
title: Maven Plugin
date: 2017-03-18
category: Maven
---

# Maven-resources-plugin

Introduction: http://maven.apache.org/plugins/maven-resources-plugin/

为了使项目结构更为清晰，Maven区别对待Java代码文件和资源文件，maven-compiler-plugin用来编译Java代码，**maven-resources-plugin**则用来处理资源文件。默认的主资源文件目录是src/main/resources，很多用户会需要添加额外的资源文件目录，这个时候就可以通过配置**maven-resources-plugin**来实现。此外，资源文件过滤也是Maven的一大特性，你可以在资源文件中使用${propertyName}形式的Maven属性，然后配置**maven-resources-plugin**开启对资源文件的过滤，之后就可以针对不同环境通过命令行或者Profile传入属性的值，以实现更为灵活的构建。

另外，对于resouce的编码，如果不显示设置，Maven会使用平台的默认编码，中文系统就是GBK。所以如果项目中统一使用UTF-8编码，可参考一下配置：
```
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-resources-plugin</artifactId>
  <version>2.7</version>
  <configuration>
    <encoding>UTF-8</encoding>
  </configuration>
</plugin>
```
