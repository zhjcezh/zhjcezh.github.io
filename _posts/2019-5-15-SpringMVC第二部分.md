---
layout:     post
title:      "SpringMVC第二部分.md"
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

## 注解的处理器映射器和适配器   
在spring3.1之前使用org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping注解映射器。    

在spring3.1之后使用org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping注解映射器。   

在spring3.1之前使用org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter注解适配器。    

在spring3.1之后使用org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter注解适配器。    

#### 配置注解映射器和适配器    

```   
<!--注解映射器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
<!--注解适配器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>

<!-- 使用 mvc:annotation-driven代替上边注解映射器和注解适配器配置
	mvc:annotation-driven默认加载很多的参数绑定方法，
	比如json转换解析器就默认加载了，如果使用mvc:annotation-driven不用配置上边的RequestMappingHandlerMapping和RequestMappingHandlerAdapter
	实际开发时使用mvc:annotation-driven
-->
<mvc:annotation-driven></mvc:annotation-driven>
```   

#### 开发注解Handler   
使用注解的映射器和注解的适配器。（注解的映射器和注解的适配器必须配对使用）

```
//使用Controller标识 它是一个控制器
@Controller
public class ItemsController3 {
	//商品查询列表
	//@RequestMapping实现 对queryItems方法和url进行映射，一个方法对应一个url
	//一般建议将url和方法写成一样
	@RequestMapping("/queryItems")
	public ModelAndView queryItems() throws Exception{
		//调用service查找 数据库，查询商品列表，这里使用静态数据模拟
        ......
		//返回ModelAndView
		ModelAndView modelAndView =  new ModelAndView();
		//相当 于request的setAttribut，在jsp页面中通过itemsList取数据
		modelAndView.addObject("itemsList", itemsList);
		
		//指定视图
		modelAndView.setViewName("/WEB-INF/jsp/items/itemsList.jsp");
	
		return modelAndView;
		
	}

```

#### 在spring容器中加载Handler    
```  
    <!-- 对于注解的Handler可以单个配置
	实际开发中建议使用组件扫描
	 -->
	<!-- <bean class="cn.itcast.ssm.controller.ItemsController3" /> -->
	<!-- 可以扫描controller、service、...
	这里让扫描controller，指定controller的包
	 -->
	<context:component-scan base-package="cn.itcast.ssm.controller"></context:component-scan>
```  

## 小结  
通过入门程序理解springmvc前端控制器、**处理器映射器、处理器适配器**、视图解析器用法。     

前端控制器配置：   
第一种：*.action，访问以.action结尾 由DispatcherServlet进行解析    
     
第二种：/，所以访问的地址都由DispatcherServlet进行解析，对于静态文件的解析需要配置不让DispatcherServlet进行解析   
  	使用此种方式可以实现 RESTful风格的url    

处理器映射器：    
非注解处理器映射器（了解）   
注解的处理器映射器（掌握）    
	对标记@Controller类中标识有@RequestMapping的方法进行映射。在@RequestMapping里边定义映射的url。使用注解的映射器不用在xml中配置url和Handler的映射关系。     

处理器适配器：    
非注解处理器适配器（了解）    
注解的处理器适配器（掌握）     
	注解处理器适配器和注解的处理器映射器是配对使用。理解为不能使用非注解映射器进行映射。      

<mvc:annotation-driven></mvc:annotation-driven>可以代替下边的配置：     

> 	<!--注解映射器 -->       
> 	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>        
> 	<!--注解适配器 -->         
> 	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>        
       
实际开发使用：mvc:annotation-driven     

视图解析器配置前缀和后缀：   
```   
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



//指定视图
//下边的路径，如果在视图解析器中配置jsp路径的前缀和jsp路径的后缀，修改为
//modelAndView.setViewName("/WEB-INF/jsp/items/itemsList.jsp");
//上边的路径配置可以不在程序中指定jsp路径的前缀和jsp路径的后缀
modelAndView.setViewName("items/itemsList");
```   
