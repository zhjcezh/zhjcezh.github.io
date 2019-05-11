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
