---
layout:     post
title:      "SpringMVC第二部分"
subtitle:   ""
date:       2019-05-12 14:00:00
author:     "Cfeng"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
- SSM框架
- Java
- 学习之路
---
学习视频来自黑马程序员，感谢！ 

## springmvc和mybatis整合     

#### 需求   
使用springmvc和mybatis完成商品列表查询。    
     

#### 整合思路    
springmvc+mybaits的系统架构：表现层springmvc-->业务层service-->持久层mybaits。       
spring将各层进行整合    
通过spring管理持久层的mapper(相当于dao接口)      
通过spring管理业务层service，service中可以调用mapper接口。     
spring进行事务控制。      
通过spring管理表现层Handler，Handler中可以调用service接口。     
mapper、service、Handler都是javabean。      
      
      
第一步：整合dao层      
mybatis和spring整合，通过spring管理mapper接口。       
使用mapper的扫描器自动扫描mapper接口在spring中进行注册。      

第二步：整合service层      
通过spring管理 service接口。      
使用配置方式将service接口配置在spring配置文件中。       
实现事务控制。       

第三步：整合springmvc层      
由于springmvc是spring的模块，不需要整合。      

#### 整合dao    
mybatis和spring进行整合。       
1.sqlMapConfig.xml      
mybaits自己的配置文件。     
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 全局setting配置，根据需要添加 -->
    <!-- 配置别名 -->
    <typeAliases>
        <!-- 批量扫描别名 -->
        <package name="cn.itcast.ssm.po"/>
    </typeAliases>
    <!-- 配置mapper
    由于使用spring和mybatis的整合包进行mapper扫描，这里不需要配置了。
    必须遵循：mapper.xml和mapper.java文件同名且在一个目录 
    -->
    <!-- <mappers>
    </mappers> -->
</configuration>
```    
        
2.applicationContext-dao.xml       
配置：     
数据源      
SqlSessionFactory      
mapper扫描器      
```
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
xsi:schemaLocation="http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
http://www.springframework.org/schema/mvc 
http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
http://www.springframework.org/schema/context 
http://www.springframework.org/schema/context/spring-context-3.2.xsd 
http://www.springframework.org/schema/aop 
http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
http://www.springframework.org/schema/tx 
http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">

    <!-- 加载db.properties文件中的内容，db.properties文件中key命名要有一定的特殊规则 -->
    <context:property-placeholder location="classpath:db.properties" />
    <!-- 配置数据源 ，dbcp --> 
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
    destroy-method="close">
        <property name="driverClassName" value="${jdbc.driver}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <property name="maxActive" value="30" />
        <property name="maxIdle" value="5" />
    </bean>
    <!-- sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 加载mybatis的全局配置文件 -->
        <property name="configLocation" value="classpath:mybatis/sqlMapConfig.xml" />
    </bean>
    <!-- mapper扫描器 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 扫描包路径，如果需要扫描多个包，中间使用半角逗号隔开 -->
        <property name="basePackage" value="cn.itcast.ssm.mapper"></property>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
    </bean>
</beans>
```     
3.逆向工程生成po类及mapper(单表增删改查)        
生成代码配置文件     
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
    <commentGenerator>
        <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
        <property name="suppressAllComments" value="true" />
    </commentGenerator>

    <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
        connectionURL="jdbc:mysql://localhost:3306/mybatis" userId="root"
        password="*******">
    </jdbcConnection>

    <!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
    connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg" 
    userId="yycg"
    password="yycg">
    </jdbcConnection> -->

    <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和 
    NUMERIC 类型解析为java.math.BigDecimal -->
    <javaTypeResolver>
        <property name="forceBigDecimals" value="false" />
    </javaTypeResolver>

    <!-- targetProject:生成PO类的位置 -->
    <javaModelGenerator targetPackage="com.cfeng.ssm.po"
        targetProject="./src">
        <!-- enableSubPackages:是否让schema作为包的后缀 -->
        <property name="enableSubPackages" value="false" />
        <!-- 从数据库返回的值被清理前后的空格 -->
        <property name="trimStrings" value="true" />
    </javaModelGenerator>
    <!-- targetProject:mapper映射文件生成的位置 -->
    <sqlMapGenerator targetPackage="com.cfeng.ssm.mapper"
        targetProject="./src">
        <!-- enableSubPackages:是否让schema作为包的后缀 -->
        <property name="enableSubPackages" value="false" />
    </sqlMapGenerator>
    <!-- targetPackage：mapper接口生成的位置 -->
    <javaClientGenerator type="XMLMAPPER"
        targetPackage="com.cfeng.ssm.mapper"
        targetProject="./src">
        <!-- enableSubPackages:是否让schema作为包的后缀 -->
        <property name="enableSubPackages" value="false" />
    </javaClientGenerator>

        <!-- 指定数据库表 -->
        <table tableName="items"></table>
        <table tableName="orders"></table>
        <table tableName="orderdetail"></table>
        <table tableName="user"></table>

        <!-- <table schema="" tableName="sys_user"></table>
        <table schema="" tableName="sys_role"></table>
        <table schema="" tableName="sys_permission"></table>
        <table schema="" tableName="sys_user_role"></table>
        <table schema="" tableName="sys_role_permission"></table> -->

        <!-- 有些表的字段需要指定java类型
        <table schema="" tableName="">
            <columnOverride column="" javaType="" />
        </table> -->
    </context>
</generatorConfiguration>
```

注意
**windows 下的路径是.\src**
**mac下路径是./src**  


执行生成程序      
```
import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.exception.XMLParserException;
import org.mybatis.generator.internal.DefaultShellCallback;

public class GeneratorSqlmap {
    public void generator() throws Exception{
            List<String> warnings = new ArrayList<String>();
            boolean overwrite = true;
            //指定 逆向工程配置文件
            File configFile = new File("generatorConfig.xml"); 
            ConfigurationParser cp = new ConfigurationParser(warnings);
            Configuration config = cp.parseConfiguration(configFile);
            DefaultShellCallback callback = new DefaultShellCallback(overwrite);
            MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
            callback, warnings);
            myBatisGenerator.generate(null);
        } 
        public static void main(String[] args) throws Exception {
        try {
            GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
            generatorSqlmap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
注意配置文件的路径问题，不能放在resource文件，不然会报找不到文件的错误，需放在src目录下，或与它同级。   
放在src目录下的路径为src/generatorConfig.xml       
     
4.手动定义商品查询mapper       
针对综合查询mapper，一般情况会有关联查询，建议自定义mapper      
       
       
#### 整合service      

让spring管理service接口。       
```    
public interface ItemsService {
    //商品查询列表
    public List<ItemsCustom> findItemsList(ItemsQueryVo itemsQueryVo) throws Exception;
}
```       
```     
public class ItemsServiceImpl implements ItemsService{
    @Autowired
    private ItemsMapperCustom itemsMapperCustom;

    @Autowired
    private ItemsMapper itemsMapper;

    @Override
    public List<ItemsCustom> findItemsList(ItemsQueryVo itemsQueryVo)
    throws Exception {
        //通过ItemsMapperCustom查询数据库
        return itemsMapperCustom.findItemsList(itemsQueryVo);
    }
}
```     
        
2.在spring容器配置service(applicationContext-service.xml)           
创建applicationContext-service.xml，文件中配置service。      
```    
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
xsi:schemaLocation="http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
http://www.springframework.org/schema/mvc 
http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
http://www.springframework.org/schema/context 
http://www.springframework.org/schema/context/spring-context-3.2.xsd 
http://www.springframework.org/schema/aop 
http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
http://www.springframework.org/schema/tx 
http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">
    <!-- 商品管理的service -->
    <bean id="itemsService" class="cn.itcast.ssm.service.impl.ItemsServiceImpl"/>
</beans>
```    
       
3.事务控制(applicationContext-transaction.xml)      
在applicationContext-transaction.xml中使用spring声明式事务控制方法。       
```   
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
xsi:schemaLocation="http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans-3.2.xsd 
http://www.springframework.org/schema/mvc 
http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd 
http://www.springframework.org/schema/context 
http://www.springframework.org/schema/context/spring-context-3.2.xsd 
http://www.springframework.org/schema/aop 
http://www.springframework.org/schema/aop/spring-aop-3.2.xsd 
http://www.springframework.org/schema/tx 
http://www.springframework.org/schema/tx/spring-tx-3.2.xsd ">

    <!-- 事务管理器 
    对mybatis操作数据库事务控制，spring使用jdbc的事务控制类
    -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 数据源
        dataSource在applicationContext-dao.xml中配置了
        -->
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 通知 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <!-- 传播行为 -->
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
            <tx:method name="get*" propagation="SUPPORTS" read-only="true"/>
            <tx:method name="select*" propagation="SUPPORTS" read-only="true"/>
        </tx:attributes>
    </tx:advice>
    <!-- aop -->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* cn.itcast.ssm.service.impl.*.*(..))"/>
    </aop:config>

</beans>
```      
     
#### 整合springmvc     
1.springmvc.xml      
```    
<!-- 可以扫描controller、service、...
这里让扫描controller，指定controller的包
-->
<context:component-scan base-package="cn.itcast.ssm.controller"></context:component-scan>

<mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
<!-- 视图解析器
解析jsp解析，默认使用jstl标签，classpath下的得有jstl的包
-->
<bean
    class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!-- 配置jsp路径的前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!-- 配置jsp路径的后缀 -->
    <property name="suffix" value=".jsp"/>
</bean>
``` 

2.前端控制器     
```    
<!-- springmvc前端控制器 -->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- contextConfigLocation配置springmvc加载的配置文件（配置处理器映射器、适配器等等） 如果不配置contextConfigLocation，默认加载的是/WEB-INF/servlet名称-serlvet.xml（springmvc-servlet.xml） -->
    <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring/springmvc.xml</param-value>
    </init-param>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!-- 第一种：*.action，访问以.action结尾 由DispatcherServlet进行解析 第二种：/，所以访问的地址都由DispatcherServlet进行解析，对于静态文件的解析需要配置不让DispatcherServlet进行解析
    使用此种方式可以实现 RESTful风格的url 第三种：/*，这样配置不对，使用这种配置，最终要转发到一个jsp页面时， 仍然会由DispatcherServlet解析jsp地址，不能根据jsp页面找到handler，会报错。 -->
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```     
3.编写Controller(就是Handler)     
```  
@Controller
public class ItemsController {

    @Autowired
    private ItemsService itemsService;

    // 商品查询
    @RequestMapping("/queryItems")
    public ModelAndView queryItems(HttpServletRequest request) throws Exception {
        //测试forward后request是否可以共享

        System.out.println(request.getParameter("id"));

        // 调用service查找 数据库，查询商品列表
        List<ItemsCustom> itemsList = itemsService.findItemsList(null);

        // 返回ModelAndView
        ModelAndView modelAndView = new ModelAndView();
        // 相当 于request的setAttribut，在jsp页面中通过itemsList取数据
        modelAndView.addObject("itemsList", itemsList);
        // 指定视图
        modelAndView.setViewName("items/itemsList");
        return modelAndView;
    }
}
```     
4.编写jsp      
        
#### 加载spring容器    
将mapper、service、controller加载到spring容器中。    
建议使用通配符加载上边的配置文件。     
      
在web.xml中，添加spring容器监听器，加载spring容器。      
```
<!-- 加载spring容器 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring/applicationContext-*.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
