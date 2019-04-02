---
layout:     post
title:      "ModelDriven模型驱动中文乱码问题"
subtitle:   ""
date:       2019-03-13 12:00:00
author:     "Cfeng"
header-img: "img/home-bg.jpg"
catalog: true
tags:
    - bug
    - struts2
---
在使用struts做项目的时候,使用ModelDriven接收传进来的中文时，老是出现乱码。一直没有去解决。然后最近发现只是tomcat配置问题。
当传递参数的时候发生乱码，需要修改tomcat服务器server.xml文件在Connector节点中加入 URIEncoding=”UTF-8”就可以了如下：
```
<Connector port="8080" protocol="HTTP/1.1"
   connectionTimeout="20000"
   redirectPort="8443"
   URIEncoding="UTF-8"
/>
```
而一般提交数据是使用post方法提交有时还会乱码那么除了上一步，还应做第二步，在struts.xml中配置编码即：
```
<constant name="struts.i18n.encoding" value="GBK"
```