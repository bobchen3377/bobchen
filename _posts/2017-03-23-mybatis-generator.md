---
layout: post
title: Mybatis Generator - Mapper, Pojo Auto-Generation
date: 2017-03-23
category: Mybatis
---

### 序言

Mybatis属于半自动的ORM(Object Relational Mapping)， 而在为对应的数据库手动创建Mapper、Pojo需要不小的工作量，而且容易出错。
因此， 我们有必要使用Mybatis-Generator来帮助我们实现Mapper, Pojo auto-generation。

### 1. Download Jars  

#### **1.1) Download mybatis-generator-core-1.3.5.jar**

Link: <a href="http://repo1.maven.org/maven2/org/mybatis/generator/mybatis-generator-core/"  target="_blank">
http://repo1.maven.org/maven2/org/mybatis/generator/mybatis-generator-core/</a>  

#### **1.2) Download mysql-connector-java-5.1.39.jar**

Link: <a href="http://repo1.maven.org/maven2/mysql/mysql-connector-java/" target="_blank">
http://repo1.maven.org/maven2/mysql/mysql-connector-java/</a> 

---

### 2. Setup Xml  
#### **2.1) 创建 generator.xml**
所有jar和文件都放在 `C:\Users\Bob\Desktop\Test\Mybatis_Test` 目录下
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<!-- 数据库驱动包位置 -->
	<classPathEntry location="C:\Users\Bob\Desktop\Test\Mybatis_Test\mysql-connector-java-5.1.39.jar" />
	<context id="DB2Tables" targetRuntime="MyBatis3">
		<commentGenerator>
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!-- 数据库链接URL、用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/mybatistest?useSSL=false" userId="u"
		password="password">
		</jdbcConnection>
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>
		<!-- 生成模型的包名和位置 -->
		<javaModelGenerator targetPackage="bob.model" targetProject="C:\Users\Bob\Desktop\Test\Mybatis_Test\src">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!-- 生成的映射文件包名和位置 -->
		<sqlMapGenerator targetPackage="bob.mapping" targetProject="C:\Users\Bob\Desktop\Test\Mybatis_Test\src">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		<!-- 生成DAO的包名和位置 -->
		<javaClientGenerator type="XMLMAPPER" targetPackage="bob.dao" targetProject="C:\Users\Bob\Desktop\Test\Mybatis_Test\src">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		<!-- 要生成那些表(更改tableName和domainObjectName就可以) -->
		<table tableName="students" domainObjectName="Student" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false"
		enableSelectByExample="false" selectByExampleQueryId="false" />
		<table tableName="teachers" domainObjectName="Teacher" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false"
		enableSelectByExample="false" selectByExampleQueryId="false" />
	</context>
</generatorConfiguration>
```
* **要注意修改路径 C:\Users\Bob\Desktop\Test\Mybatis_Test**

* **这是测试数据库mybatistest的表**
```
CREATE TABLE `students` (
  `sno` varchar(12) NOT NULL,
  `sname` varchar(20) DEFAULT NULL,
  `spwd` varchar(15) DEFAULT NULL,
  `sclass` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`sno`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
```
CREATE TABLE `teachers` (
  `tno` varchar(20) NOT NULL,
  `tname` varchar(20) DEFAULT NULL,
  `tpwd` varchar(15) DEFAULT NULL,
  `tclass` varchar(1000) DEFAULT NULL,
  `position` int(11) DEFAULT NULL,
  PRIMARY KEY (`tno`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### **2.2) 在 `C:\Users\Bob\Desktop\Test\Mybatis_Test` 下创建src文件夹**

* **完成以上后， 路径下应该包括这些：**
<img src="\assets\images\mybatis-generator\mybatis-generator-1.png" width="800" />

---

### 3. Execute command to generate source  
在 `C:\Users\Bob\Desktop\Test\Mybatis_Test` 下，  
"Shift" + 鼠标右键 -> Open command window here
<img src="\assets\images\mybatis-generator\mybatis-generator-2.png" width="800" />

执行Command:   
`java -jar mybatis-generator-core-1.3.5.jar -configfile generator.xml -overwrite`
<img src="\assets\images\mybatis-generator\mybatis-generator-3.png" width="800" />
可以看到`Mybatis Generator finished successfully.`， 代表执行成功

---

### 4. Check the result  
可以看到有dao, mapping, model：  
<img src="\assets\images\mybatis-generator\mybatis-generator-4.png" width="600" />

<img src="\assets\images\mybatis-generator\mybatis-generator-5.png" width="600" />

<img src="\assets\images\mybatis-generator\mybatis-generator-6.png" width="600" />

<img src="\assets\images\mybatis-generator\mybatis-generator-7.png" width="600" />
