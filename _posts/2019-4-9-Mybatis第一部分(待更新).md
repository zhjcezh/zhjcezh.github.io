---
layout:     post
title:      "Mybatis第一部分"
subtitle:   "每天学习一点点"
date:       2019-04-09 19:00:00
author:     "Cfeng"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - Java
    - SSM框架
    - 学习之路
---
学习视频来自黑马程序员，感谢！


# Mybatis第一部分
## 对原生态jdbc程序问题总结
1. 环境
java环境：jdk1.8
开发环境：IDEA
数据库  ：mysql5.7

2. jdbc程序
使用jdbc查询mysql数据库中用户表的记录。
```
package cn.itcast.mybatis.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class JdbcTest {
    
    public static void main(String[] args) {
        
        //数据库连接
        Connection connection = null;
        //预编译的Statement，使用预编译的Statement提高数据库性能
        PreparedStatement preparedStatement = null;
        //结果 集
        ResultSet resultSet = null;
        
        try {
            //加载数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            
            //通过驱动管理类获取数据库链接
            connection =  DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8", "root", "mysql");
            //定义sql语句 ?表示占位符
            String sql = "select * from user where username = ?";
            //获取预处理statement
            preparedStatement = connection.prepareStatement(sql);
            //设置参数，第一个参数为sql语句中参数的序号（从1开始），第二个参数为设置的参数值
            preparedStatement.setString(1, "王五");
            //向数据库发出sql执行查询，查询出结果集
            resultSet =  preparedStatement.executeQuery();
            //遍历查询结果集
            while(resultSet.next()){
                System.out.println(resultSet.getString("id")+"  "+resultSet.getString("username"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            //释放资源
            if(resultSet!=null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
            if(preparedStatement!=null){
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
            if(connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
        }
    }
}

```
3. 问题
* 数据库连接，使用时就创建，不适用立即释放，对数据库进行频繁连接开启和关闭，造成数据库资源浪费，影响数据库性能。
**解决方法**：使用数据库连接池管理数据库连接。

* Sql语句在代码中硬编码，造成代码不易维护，实际应用sql变化的可能较大，sql变动需要改变java代码。
**解决方法**：将sql语句配置在xml配置文件中，即使sql变化，不需要对java代码进行重新编译。

* 向preparedStatement中设置参数，对占位符号位置和设置参数值，硬编码在java代码中，不利于系统维护。
**解决方法**：将sql语句及占位符和参数全部配置在xml中

* 从result中遍历结果集数据时，存在硬编码，将获取表对字段进行硬编码，不利于系统维护。
**解决方法**：将查询结果集，自动映射成java对象。 
    
    
## Mybatis框架原理
#### Mybatis是什么
* 是一个持久层的框架，是apache下的顶级项目
* 让程序员将主要精力放在sql。通过Mybatis提供的映射方式。半自动化生成满足需要sql。
* 可以将向preparedStatement中的输入参数自动进行输入映射，将查询结果集灵活映射出java对象。
#### Mybatis框架
1. SqlMapConfig.xml(是Mybatis的全局配置文件)
    配置了数据项、事物等Mybatis运行环境
2. 多个mapper.xml(配置映射关系、sql语句)
3. SqlSessionFactory(会话工厂)根据配置文件创建工厂
    作用：创建SqlSession
4. SqlSeesion(会话) 是一个接口，面向用户
    作用：操作数据库(发出sql增删改查)
5. Executor(执行器)，是一个接口（基本执行器、缓存执行器），
    作用：SqlSession内部通过执行器操作数据库
6. mapped statement(底层分装对象)
    作用：对操作数据库存储封装，包括sql语句、输入、输出结果类型
     
## Mybatis入门程序
>需求：
根据用户id（主键）查询用户信息
根据用户名称模糊查询用户信息
添加用户
删除用户
更新用户

### 创建SqlMapConfig.xml与映射文件User.xml
SqlMapConfig.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 加载属性文件 -->
    <properties resource="db.properties">
        <!--properties中还可以配置一些属性名和属性值  -->
        <!-- <property name="jdbc.driver" value=""/> -->
    </properties>
    <typeAliases>
        <!-- 针对单个别名定义
        type：类型的路径
        alias：别名
        -->
        <!-- <typeAlias type="cn.itcast.mybatis.po.User" alias="user"/> -->
        <!-- 批量别名定义
        指定包名，mybatis自动扫描包中的po类，自动定义别名，别名就是类名（首字母大写或小写都可以）
        -->
        <package name="com.caoerzhe.pojo"/>
    </typeAliases>

    <!-- 和spring整合后 environments配置将废除-->
    <environments default="development">
        <environment id="development">
            <!-- 使用jdbc事务管理，事务控制由mybatis-->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池，由mybatis管理-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}" />
                <property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.username}" />
                <property name="password" value="${jdbc.password}" />
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="sqlmap/User.xml"/>
    </mappers>
</configuration>
```
User.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace命名空间，作用就是对sql进行分类化管理，理解sql隔离
注意：使用mapper代理方法开发，namespace有特殊重要的作用
-->
<mapper namespace="test">

    <!-- 在 映射文件中配置很多sql语句 -->
    <!-- 需求：通过id查询用户表的记录 -->
    <!-- 通过 select执行数据库查询
    id：标识 映射文件中的 sql
    将sql语句封装到mappedStatement对象中，所以将id称为statement的id
    parameterType：指定输入 参数的类型，这里指定int型
    #{}表示一个占位符号
    #{id}：其中的id表示接收输入 的参数，参数名称就是id，如果输入 参数是简单类型，#{}中的参数名可以任意，可以value或其它名称
    resultType：指定sql输出结果 的所映射的java对象类型，select指定resultType表示将单条记录映射成的java对象。
    -->
    <select id="findUserById" parameterType="int" resultType="com.caoerzhe.pojo.User">
        SELECT * FROM user WHERE id = #{id}
    </select>
</mapper>
```

### 根据用户id（主键）查询用户信息

## Mybatis框架原理
## Mybatis入门程序
### 用户对增删改查
## Mybatis开发dao两种方法
### 原始dao开发方法
### Mybatis对mapper代理开发方法
## Mybatis配置文件
## Mybatis核心
### Mybatis输入映射
### Mybatis输出映射
## Mybatis对动态sql


