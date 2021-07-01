---
layout:     post
title:      feign整合sentinel
subtitle:   "spring cloud alibaba 学习"
date:       2021-7-1 11:00:00
author:     "Cfeng"
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - spring cloud
---

# Sentinel 控制台启动
Sentinel 控制台提供一个轻量级的控制台，它提供机器发现、单机资源实时监控、集群资源汇总，以及规则管理的功能。只需要对应用进行简单的配置，就可以使用这些功能。

注意: 集群资源汇总仅支持**500** 台以下的应用集群，有大概 1 - 2 秒的延时。

```
# 下载源码
git clone https://github.com/alibaba/Sentinel.git

# 编译打包
mvn clean package

# 找到sentinel-dashboard.jar然后启动
java -Dserver.port=8080 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard.jar
```
如若 8080 端口冲突，可使用 **-Dserver.port=新端口** 进行设置。


打开浏览器访问：http://localhost:8080/#/dashboard/home

# feign整合Sentinel

引入依赖并打开开关
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>


feign.sentinel.enabled:true


spring.cloud.sentinel.transport.port:8720
spring.cloud.sentinel.transport.dashboard:localhost:8080
```

在存根接口中，配置熔断后调用的实现类,注意@Component
```
@FeignClient(value = "nacos-provider", fallback = EchoServiceFallback.class)
public interface EchoService {

    @GetMapping(value = "/echo/{message}")
    String echo(@PathVariable("message") String message);
}

@Component
public class EchoServiceFallback implements EchoService {
    @Override
    public String echo(String message) {
        return "echo fallback";
    }
}
```

之后请求熔断，都能在控制台中看到

