---
layout: post
title: Maven Plugins
date: 2017-03-18
category: Maven
---

所有的Maven Plugins可参考地址：<a href="http://maven.apache.org/plugins/index.html" target="_blank">http://maven.apache.org/plugins/index.html</a>  

### maven-resources-plugin  
Introduction: <a href="http://maven.apache.org/plugins/maven-resources-plugin/" target="_blank">http://maven.apache.org/plugins/maven-resources-plugin/</a>

为了使项目结构更为清晰，Maven区别对待Java代码文件和资源文件，maven-compiler-plugin用来编译Java代码，**maven-resources-plugin**则用来处理资源文件。  
默认的主资源文件目录是src/main/resources，很多用户会需要添加额外的资源文件目录，这个时候就可以通过配置**maven-resources-plugin**来实现。  
此外，资源文件过滤也是Maven的一大特性，你可以在资源文件中使用 `${propertyName}` 形式的Maven属性，然后配置**maven-resources-plugin**开启对资源文件的过滤，之后就可以针对不同环境通过命令行或者Profile传入属性的值，以实现更为灵活的构建。

另外，对于resouce的编码，如果不显式设置，Maven会使用平台的默认编码，中文系统就是GBK。  
所以如果项目中统一使用UTF-8编码，可参考以下POM配置：
```
<build>
	<plugins>
		<plugin>
		  <groupId>org.apache.maven.plugins</groupId>
		  <artifactId>maven-resources-plugin</artifactId>
		  <version>2.7</version>
		  <configuration>
		    <encoding>UTF-8</encoding>
		  </configuration>
		</plugin>
	</plugins>
</build>
```

---

### maven-compiler-plugin  
Introduction: <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">http://maven.apache.org/plugins/maven-compiler-plugin/</a>

为了使项目结构更为清晰，Maven区别对待Java代码文件和资源文件，**maven-compiler-plugin**用来编译Java代码，**maven-resources-plugin**则用来处理资源文件。

另外，为避免JDK以及编码的问题对于resouce的编码，可参考一下POM配置显式指定JDK版本以及编码：
```
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>3.2</version>
			<configuration>
				<source>1.7</source>
				<target>1.7</target>
				<encoding>UTF-8</encoding>
			</configuration>
		</plugin>
	</plugins>
</build>
```

---

### tomcat7-maven-plugin  
Introduction: <a href="http://tomcat.apache.org/maven-plugin.html" target="_blank">http://tomcat.apache.org/maven-plugin.html</a>

**tomcat7-maven-plugin**可以让Maven Web项目的WAR包部署到Tomcat上的Servlet Container变得更加方便.

**1) 简单的参考POM配置:**  
```
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<port>8080</port>
				<path>/</path>
			</configuration>
		</plugin>
	</plugins>
</build>
```  
对于**tomcat7-maven-plugin** Version2.2, 以上配置没有指定Tomcat, 就会使用Apache Tomcat/7.0.47进行本地配置，并运行WAR包.
配置好后, 右键*Project -> Run As -> Maven Build -> Goals*, 输入`clean tomcat7:run`, 然后run, 成功后项目就会部署切运行在 `localhost:8080`.

**2) 指定Tomcat的POM配置:**  
```
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<url>http://192.168.1.107:8080/manager/text</url>
				<username>tomcat</username>
				<password>password</password>
			</configuration>
		</plugin>
	</plugins>
</build>
```  
当然, 在ip为`192.168.1.107`的Linux虚拟机上, 需要配置Tomcat并运行在8080端口:  
`vim /usr/local/tomcat1/conf/tomcat-users.xml`, 找到 `<tomcat-users></tomcat-users>` 属性, 在里面加入以下配置:  
```
<tomcat-users>
	<role rolename="manager-gui" />
	<role rolename="manager-script" />
	<user username="tomcat" password="password" roles="manager-gui, manager-script"/>
</tomcat-users>
```  
配置好后, 在eclipse右键*Project -> Run As -> Maven Build -> Goals*, 第一次run用`clean tomcat7:deploy`, 以后用`clean tomcat7:redeploy`,  
然后run, 成功后项目就会部署并运行在 `192.168.1.107:8080`.