# JDBCLearn

一个关于学习使用JDBC进行数据库基本操作的Demo

## 0.写在前面

发现许多身边的同学，在分别学习了Java和数据库相应课程之后，没有意识到或者不知道该如何将两者结合起来应用，导致不能很好地将所学知识运用到实际项目中。
JDBC作为Java访问数据库的应用程序接口，是Java程序员必须掌握的技能之一，也是Mybatis等框架的基础。
本项目通过一个简单的Demo，演示了如何用JDBC对MySQL数据库进行基本增删改查操作，以及将数据库表与实体类映射的ORM思想基础，希望能帮助到学习中的大家。

## 1.技术栈

- Java：一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级Web应用开发和移动应用开发。
- MySQL：一个关系型数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle公司。
- JDBC：Java语言中用来规范客户端程序如何来访问数据库的应用程序接口，提供了诸如查询和更新数据库中数据的方法。
- Druid：是阿里巴巴开源平台上一个数据库连接池实现，它结合了C3P0、DBCP、Proxool等DB池的优点，同时加入了日志监控。

## 2. 项目简介

- 使用MySQL提供的JDBC驱动程序进行数据库连接，通过JDBC对MySQL数据库进行基本的增删改查操作。
- 使用Druid提供的数据库连接池实现，对数据库连接进行管理，提高数据库访问效率。
- 项目分为`JDBCDemo.java`及`BrandTest.java`两个文件，分别对应JDBC的基本操作的演示和对使用JDBC进行`Brand`实体类数据的增删改查操作的演示。
- 为降低学习门槛，没有使用常见的Maven项目管理工具，将项目所依赖的jar包放置在`lib`文件夹中，使用IDEA将其添加到项目中。
- 为降低学习门槛，没有使用JUnit进行单元测试，将测试函数直接写在了对应测试类中，通过`main`函数进行调用，结果通过控制台输出。

## 3. 环境配置

- JDK 1.8：Java开发工具包
- MySQL 8.0.28：关系型数据库
- MySQL Connector/J 8.0.28：MySQL提供的JDBC实现
- Druid 1.2.16：阿里巴巴提供的数据库连接池实现
- IDEA 2023.1：Java集成开发环境
- HeidiSQL 12.4：数据库可视化管理工具

## 4. 怎么把项目跑起来

1. 使用`git clone`命令将本项目克隆到本地，或者直接下载本项目的压缩包。
2. 使用HeidiSQL创建本项目所使用的数据库，数据库名称为`jdbc_test`。
3. 使用IDEA打开本项目，本项目依赖包已位于`lib`文件夹中，无需再次下载。
4. 修改`JDBCDemo.java`以及`druid.properties`文件中的数据库连接信息，包括数据库地址、用户名和密码等。
5. 运行文件，查看运行结果。

## 5. 相关配置

- 本项目所使用的数据库名称为`jdbc_test`，其中包含三张表，分别为`product`、`tb_brand`、`tb_user`，表结构如下：

```sql
CREATE DATABASE IF NOT EXISTS `jdbc_test` /*!40100 DEFAULT CHARACTER SET utf8 */ /*!80016 DEFAULT ENCRYPTION = 'N' */;
USE `jdbc_test`;

CREATE TABLE IF NOT EXISTS `product`
(
    `id`    int NOT NULL AUTO_INCREMENT,
    `name`  varchar(20)    DEFAULT NULL,
    `price` decimal(10, 2) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 6
  DEFAULT CHARSET = utf8mb3;

INSERT INTO `product` (`id`, `name`, `price`)
VALUES (1, '商品1', 1000.00),
       (2, '商品2', 500.00),
       (3, '商品3', 800.00),
       (4, '商品4', 600.00),
       (5, '商品5', 500.00);

CREATE TABLE IF NOT EXISTS `tb_brand`
(
    `id`           int NOT NULL AUTO_INCREMENT,
    `brand_name`   varchar(20)  DEFAULT NULL,
    `company_name` varchar(20)  DEFAULT NULL,
    `ordered`      int          DEFAULT NULL,
    `description`  varchar(100) DEFAULT NULL,
    `status`       int          DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `brand_name` (`brand_name`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 21
  DEFAULT CHARSET = utf8mb3;

INSERT INTO `tb_brand` (`id`, `brand_name`, `company_name`, `ordered`, `description`, `status`)
VALUES (1, '三只松鼠', '三只松鼠股份有限公司', 5, '好吃不上火', 0),
       (2, '华为', '华为技术有限公司', 100, '华为致力于把数字世界带入每个人、每个家庭、每个组织，构建万物互联的智能世界',
        1),
       (3, '小米', '小米科技有限公司', 50, 'are you ok', 1);

CREATE TABLE IF NOT EXISTS `tb_user`
(
    `id`       int         DEFAULT NULL,
    `username` varchar(20) DEFAULT NULL,
    `password` varchar(32) DEFAULT NULL
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb3;

INSERT INTO `tb_user` (`id`, `username`, `password`)
VALUES (1, 'zhangsan', '123'),
       (2, 'lisi', '234');
```

- 本项目所使用的数据库连接池配置文件为`druid.properties`，配置内容如下：

```properties
# Database connection pool configuration file
# MySQL driver class
driverClassName=com.mysql.cj.jdbc.Driver
# Database connection address
url=jdbc:mysql://127.0.0.1:3306/jdbc_test?useSSL=false&useServerPrepStmts=true
username=root
password=henu
# Connection pool configuration
# initialSize: The number of connections created when the connection pool is initialized
initialSize=5
# maxActive: The maximum number of connections in the connection pool
maxActive=10
# maxWait: The maximum waiting time for obtaining a connection from the connection pool
maxWait=3000
```