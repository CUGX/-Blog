# JavaStep5_JDK新特性、JDBC、代理模式

## 1、JDK新特性

## 1.1、静态导入



## 1.2、自动装/拆箱



## 1.3、可变参数



## 1.4、增强for循环



## 1.5、枚举类型



## 2、JDBC

1. java连接数据库：

   加载驱动类导入jar文件，把数据库的连接和资源的关闭操作进行包装JdbcUtil

   ```java
   java.sql.DriverManager
   java.sql.Connection
   java.sql.ResultSet
   ```

   

2. 把针对表的操作进行包装 DAO

   一张表----类  对应（表-->类   列---》属性   记录---》对象)

3. java.sql.Statement===>java.sql.PreparedStatement

   java.sql.PreparedStatement ：sql语句中需要传递的值可以使用“？”占位符表示。这样可以起到预编译的效果性能较好，防止sql注入。

   

### 2.1、分页



### 2.2、事务









## 3、代理模式



