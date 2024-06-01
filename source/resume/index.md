---
title: resume
date: 2024-06-01 12:07:23
layout: resume
---

# 个人简历

- 姓名：江茂明
- 邮箱: yuzhi.jiang@foxmail.com
- 个人博客: [https://blog.anml.cn/](https://blog.anml.cn/)

## 教育背景

**广东东软学院 软件工程(本科)**

- **时间:** 2019.09 - 2023.06
- **GPA:** 3.2 / 4.0
- **主修课程:** 数据结构与算法、C、Java程序设计、高等数学、计算机组成原理、操作系统原理、互联网应用开发
- **荣誉奖项:** 数学建模竞赛国家级二等奖、大湾区杯金融数学建模竞赛本科组三等奖、团体程序设计天梯赛团队三等奖

---

## 专业技能

- **编程:** 熟悉面向对象编程范式，掌握 Java 多线程、集合、IO 等技能，了解 JVM、JMM、类加载机制
- **数据库:** 熟悉 MySQL 数据库，熟练编写 SQL 语句，了解 MySQL 索引、锁、事务，了解 Redis 非关系型数据库
- **设计模式:** 熟悉单例、工厂、装饰者设计模式，了解其他常用设计模式
- **工具:** 熟悉代码管理工具 Git和SVN、Linux 基础命令，Docker
- **技术栈:** SpringMVC、SpringBoot、SpringCloud、MyBatis、MyBatis-Plus、RabbitMQ、Elasticsearch、Docker

<br/>

---

### 工作经验

| **广东宝特智能数字化技术有限公司** **2023-02 - 至今**

**技术选型: SpringBoot、EasyExcel、EasyPoi、vue-plugins-hiprint（标签或报表打印）**

**工作内容/成果：**

- 封装报表的导出功能模块，大大减少重复代码，多字段大单表导出性能从30分钟降至10秒内，其他复杂导出性能提升100%起
- 设计实现报表的高级查询功能架构，减少繁杂乱的报表查询条件，简化代码编写，效率提高200%左右，简单易用，获得使用客户好评
- 将用户权限集成到新报表，并且可以根据设置的权限字段，同步到报表的导出文件，避免敏感数据泄
- 基于节约材料成本背景，设计并实现高价值物料**匹配切割算法**，可避免10%以上的物料废弃率
- 基于设备利用率和把控背景，设计实现基于日历、假期、加班策略的设备使用预约算法，实现设备利用最大化，设备分配精确度到秒级
- 使用vue-plugins-hiprint代替思迈特报表打印，节约成本

<br/>

## 项目经验

### OJ在线判题系统 全栈 (2024.01 - 至今)

- 这是一个OJ在线判题系统，用户可以根据题目类型、难度、标签等维度，浏览题目，在线测试运行代码，并提交解决问题的代码。系统会根据题目设置的内存限制和时间限制，按照预先准备的测试用例评判用户提交的代码，并给出相应的结果。

**技术选型:** SpringBoot、Redis、Mybatis-Flex、Vue3、Arco Design

**技术要点:**

- 对项目架构进行分层拆分，分为后端主业务模块、判题模块、代码沙箱模块，降低耦合度，提高系统的可维护性和扩展性
- 将代码沙箱作为单独模块和服务，对外提供API，加入**API签名验证**，防止恶意请求。对代码沙箱添加网络请求、文件请求、依赖包等限制，防止用户提交恶意代码或进行作弊，保证服务安全和公平
- 在实现在线运行代码，使用用户自定义添加测试用例时，添加字段用于区分是否是动态提交的代码，并使用一个表来存储在线运行代码的动态测试用例的输入和代码沙箱的输出结果，避免与提交代码和判题逻辑冲突

### 轻松阅读 Java后端 (2022.04 - 2022.08)

- 这是一款在线图书阅读类项目，用户登录后可以翻阅图书，并加入自己的喜欢列表和书架，基于 SpringCloud 生态开发的微服务实践项目，采用前后端分离架构。

**技术:** SpringCloud、Gateway、Nacos、OpenFeign、JWT、ElasticSearch、MyBatis-Plus、RabbitMQ、Redis

**职责:**

- 部署 Nacos 配置中心和服务注册中心，编写相关配置文件
- 用户模块：负责用户的登录注册接口的开发，负责用户书架、喜欢看等接口的开发
- 搜索模块：负责使用倒排索引和分词器完成通过拼音和模糊搜索等功能的开发
- 图书模块：负责使用非关系型数据库实现书本数据缓存，图书查看等相关接口的开发

**技术要点:**

- 使用 SpringCloud、Gateway、JWT 实现网关层权限处理设计，减少冗余代码
- 使用 ElasticSearch 与 IK 分词器实现拼音和模糊搜的功能
- 利用 RabbitMQ 的异步功能完成 MySQL 与 ElasticSearch 索引库的数据同步

---

## 探索/成长路径

- 在大一开始在老师的推荐下开始学习数据结构与算法并参加一些竞赛，后进入校集训队训练并开始撰写个人笔记和算法题解以分享和巩固所学知识，也使用过一些如AWS、阿里云、百度云等服务器
- 个人博客路径为CSDN、博客园、hexo博客框架(部署到github)
- 博客链接：[https://yuzhi-jiang.github.io/](https://yuzhi-jiang.github.io/)

---

## 个人总结

- 热爱技术、喜欢钻研，曾参加ACM国际大学生程序设计竞赛、团体程序设计天梯赛等算法集训队
- 多年博客记录，对新技术保持好奇心，将学习成果用于项目实践，总结形成个人笔记
- 了解 JVM、JMM、类加载机制，阅读部分Spring源码 [仓库链接: https://github.com/yuzhi-jiang/small-sprinig](https://github.com/yuzhi-jiang/small-sprinig)
- 掌握 Java 多线程、集合。熟悉单例、工厂、装饰者设计模式。[链接:https://github.com/yuzhi-jiang/gof23](https://github.com/yuzhi-jiang/gof23)
- 熟悉 MySQL 数据库，熟练编写 SQL 语句，了解 MySQL 索引、锁、事务，熟悉 Redis 非关系型数据库
- 熟悉常用技术栈如：SpringMVC、SpringBoot、SpringCloud、MyBatis-Plus、RabbitMQ
- 良好的沟通、协调能力，有较强的学习能力和抗压能力

---

**个人博客:** [https://yuzhi-jiang.github.io/](https://yuzhi-jiang.github.io/)  
**GitHub:** [https://github.com/yuzhi-jiang/small-sprinig](https://github.com/yuzhi-jiang/small-sprinig)
**GitHub:** [https://github.com/yuzhi-jiang/gof23](https://github.com/yuzhi-jiang/gof23)
