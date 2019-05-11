---
layout:     post
title:      "SpringMVC第一部分.md"
subtitle:   ""
date:       2019-05-7 14:00:00
author:     "Cfeng"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
- SSM框架
- Java
- 学习之路
---
学习视频来自黑马程序员，感谢！ 

## springmvc框架  
#### 什么是springmvc   
springmvc是spring框架的一个模块，springmvc和spring无需通过中间整合层进行整合。   
springmvc是一个基于mvc的web框架。    
     
#### springmvc框架    
第一步：发起请求到前端控制器(DispatcherServlet)。  
第二步：前端控制器请求处理器映射器(HandlerMapping)查找Handler。可以根据xml配置、注解进行查找。  
第三步：处理器映射器HandlerMapping向前端控制器返回一个执行链HandlerExecutionChain{ HandlerInterceptor1, HandlerInterceptor2, ......, **Handler**}     
第四步：前端控制器调用处理器适配器(HandlerAdapter)去执行Handler      
第五步：处理器适配器去执行Handler      
第六步：Handler执行完成给适配器返回ModelAndView      
第七步：处理器适配器向前端控制器返回ModelAndView，ModelAndView是springmvc框架的一个底层对象，包括 Model和view     
第八步：前端控制器请求视图解析器去进行视图解析，根据逻辑视图名解析成真正的视图(jsp)。   
第九步：视图解析器向前端控制器返回View     
第十步：前端控制器进行视图渲染 ，视图渲染将模型数据(在ModelAndView对象中)填充到request域。    
第十一步：前端控制器向用户响应结果       
        
         
组件：   
1、前端控制器DispatcherServlet（不需要程序员开发）   
作用：接收请求，响应结果，相当于转发器，中央处理器。    
**有了DispatcherServlet减少了其它组件之间的耦合度**。     
2、处理器映射器HandlerMapping(不需要程序员开发)     
作用：根据请求的url查找Handler     
3、处理器适配器HandlerAdapter     
作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler     
4、处理器Handler(需要程序员开发)     
注意：编写Handler时按照HandlerAdapter的要求去做，这样适配器才可以去正确执行Handler      
5、视图解析器View resolver(不需要程序员开发)     
作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）     
6、视图View(需要程序员开发jsp)     
View是一个接口，实现类支持不同的View类型（jsp、freemarker、pdf...）   

## 入门程序
####  需求
springmvc和mybaits使用一个案例（商品订单管理）。    
功能需求：商品列表查询     

#### 在web.xml中配置前端控制器     
```   
<!-- springmvc前端控制器 -->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- contextConfigLocation配置springmvc加载的配置文件（配置处理器映射器、适配器等等）
    如果不配置contextConfigLocation，默认加载的是/WEB-INF/servlet名称-serlvet.xml（springmvc-servlet.xml）
    -->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!--
    第一种：*.action，访问以.action结尾 由DispatcherServlet进行解析
    第二种：/，所以访问的地址都由DispatcherServlet进行解析，对于静态文件的解析需要配置不让DispatcherServlet进行解析
    使用此种方式可以实现 RESTful风格的url
    第三种：/*，这样配置不对，使用这种配置，最终要转发到一个jsp页面时，
    仍然会由DispatcherServlet解析jsp地址，不能根据jsp页面找到handler，会报错。
    -->
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```   

#### 配置处理器适配器springmvc.xml    
在classpath下的springmvc.xml中配置处理器适配器      
``` 
<!-- 处理器适配器 所有处理器适配器都实现 HandlerAdapter接口 -->
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter" />
```   
此适配器能执行实现 Controller接口的Handler。     

#### 开发Handler
需要实现 controller接口，才能由org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter适配器执行。      
``` 
public class ItemController1 implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //调用service查找 数据库，查询商品列表，这里使用静态数据模拟
        List<Items> itemsList = new ArrayList<>();
        //向list中填充静态数据
        
        ......

        //返回ModelAndView
        ModelAndView modelAndView =  new ModelAndView();
        //相当 于request的setAttribut，在jsp页面中通过itemsList取数据
        modelAndView.addObject("itemsList", itemsList);

        //指定视图
        modelAndView.setViewName("WEB-INF/jsp/items/itemsList.jsp");

        return modelAndView;
    }
}
```   
#### 配置Handler    
将编写Handler在spring容器加载。     
```     
<!-- 配置Handler -->
<bean id="itemsController1" name="/queryItems_test.action" class="com.caoerzhe.ssm.controller.ItemController1" />
```    
#### 配置处理器映射器     
在classpath下的springmvc.xml中配置处理器映射器       
```     
<!-- 处理器映射器 将bean的name作为url进行查找 ，需要在配置Handler时指定beanname（就是url）
所有的映射器都实现 HandlerMapping接口。
-->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />
```   
 
 #### 配置视图解析器     
 需要配置解析jsp的视图解析器。       
```   
<!-- 视图解析器
解析jsp解析，默认使用jstl标签，classpath下的得有jstl的包
-->
<bean
class="org.springframework.web.servlet.view.InternalResourceViewResolver" />
```   

## 非注解的处理器映射器和适配器
#### 非注解的处理器映射器     
       
处理器映射器：     
org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping       


另一个映射器：     
org.springframework.web.servlet.handler.SimpleUrlHandlerMapping     

```
<bean id="itemsController2" class="com.caoerzhe.ssm.controller.ItemController1" /> 

<!--简单url映射  -->
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="mappings">
        <props>
            <!-- 对itemsController1进行url映射，url是/queryItems1.action -->
            <prop key="/queryItems1.action">itemsController1</prop>
            <prop key="/queryItems2.action">itemsController2</prop>
            <prop key="/queryItems3.action">itemsController2</prop>
        </props>
    </property>
</bean>
```
多个映射器可以并存，前端控制器判断url能让哪些映射器映射，就让正确的映射器处理。    

#### 非注解的处理器适配器     

org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter      
要求编写的Handler实现 Controller接口。      

org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter       
要求编写的Handler实现 HttpRequestHandler接口。      
```
//使用此方法可以通过修改response，设置响应的数据格式，比如响应json数据
/*
response.setCharacterEncoding("utf-8");
response.setContentType("application/json;charset=utf-8");
response.getWriter().write("json串");
*/
```   

