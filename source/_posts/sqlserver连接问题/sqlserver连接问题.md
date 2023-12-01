---
title: sqlserver连接问题
categories:
  - sqlserver连接问题
tags: 排查
date: 2023-12-01 13:14:56
---
## 背景

在公司有一个plm的项目，使用的是jdk1.8.0_66，有个需求是需要在plm项目中引入

**SQLServer** 与**PostgreSQL**组成两个数据源，因为项目之前引入过SQLServer，便直接将该依赖引入，然后跑起来，不出意外的确实出意外了。所以便记录了这次排查过程

<br/>

### 问题：

#### 在plm中引用了sqlServer版本10.2.1.jre8版本

```
<dependency> 
  <groupId>com.microsoft.sqlserver</groupId> 
  <artifactId>mssql-jdbc</artifactId> 
  <version>10.2.1.jre8</version> 
</dependency> 
```

#### 配置如下：

```
url: jdbc:sqlserver//ip:1433;databaseName=test;
driver-class-name:com.microsoft.sqlserver.jdbc.SQLServerDriver
```

#### 启动会有如下报错：

```
'PKIX path building failed: 
sun.security.provider.certpath.SunCertPathBuilderException: 
unable to find valid certification path to requested target'. 
ClientConnectionId:85411829-6853-4fdb-9373-b4c93e1d5e8f
```

![截图](0a3be8d07a4a075f27f1f6382d8a0891.png)

![截图](3acbd3b976f532e2078885f61486df74.png)

<br/>

### 问题分析

[PKIX 路径构建失败 - 无法找到请求目标的有效证书路径](https://techcommunity.microsoft.com/t5/azure-database-support-blog/pkix-path-building-failed-unable-to-find-valid-certification/ba-p/2591304)

[stackoverflow分析](https://stackoverflow.com/questions/21076179/pkix-path-building-failed-and-unable-to-find-valid-certification-path-to-requ)

[关于OpenJDK禁用TLS 1.0与1.1的分析](https://github.com/Tencent/TencentKona-8/wiki/%E5%85%B3%E4%BA%8EOpenJDK%E7%A6%81%E7%94%A8TLS-1.0%E4%B8%8E1.1%E7%9A%84%E5%88%86%E6%9E%90)

[JDBC Driver 10.2 for SQL Server Released](https://techcommunity.microsoft.com/t5/sql-server-blog/jdbc-driver-10-2-for-sql-server-released/ba-p/3100754)

1.openJdk8_291后连接sqlserver需要ssl安全连接，如果通过 AAD 身份验证连接时客户端计算机上缺少所需的证书，就会报错。

**在项目中，查看了jdk版本是1.8.0_66**

![截图](88d21a6da9a04aac1705266e92c5b221.png)

测试使用了另一个jdk版本**1.8.0_331**

![截图](f3ce4db5f1d0d8d620191dd0cb40fcf1.png)

可以正常启动

**猜测可能是版本比较老，依然允许tls1.0和tls1.1版本,在搜索资料时没有找到oraclejdk禁用tls1.1的信息**

<br/>

**在第四个文档中指出了，sqlserver从10.2 引入了默认情况下应用的重大更改Encrypt=true**

![截图](c8ecc473578266bb41101a7fb8809359.png)

### 解决方法

1. 在此项目中，可以通过使用jdk1.8.0_331 **此项目测试可行**
2. url参数中添加 encrypt=true;trustServerCertificate=true **此项目测试可行**
3. 修改sqlServer驱动到10.2以下 如：**9.4.1.jre8**  **此项目测试可行**
4. 将jre中的java.security的禁用算法(路径为JAVA_HOME/jre/lib/security/java.security)  将TLSv1和TLSv1.1删除 **此方法没试，此系统中查看jre并没禁用**

<br/>

![1edaf951e3fb25cb1f5d60a2eede9ede.png](1edaf951e3fb25cb1f5d60a2eede9ede.png)
