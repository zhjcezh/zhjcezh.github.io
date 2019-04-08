---
layout:     post
title:      "java发送短信SMS"
subtitle:   "做项目学习的小功能"
date:       2019-04-08 19:00:00
author:     "Cfeng"
header-img: "img/post-bg-rwd.jpg"
catalog: true
tags:
    - Java
    - 学习之路
---
# java发送短信SMS

1. 在中国网建中注册用户：本程序是通过中国网建提供的SMS短信平台实现的，该平台新用户注册可以拥有免费5条普通短信和3条彩信，足够进行尝试和体验了。[中国网建注册地址](http://sms.webchinese.cn/reg.shtml)

2. 修改短信签名：注册成功后登陆，用户登陆有首先要修改短信签名，因为中国网建中规定了，发送的短信如果没有正规的签名是不能成功发送的。

3. 修改验证码网关和绑定短信模板：如果开发的短信是为了发送验证码、订单号等需要让用户快速收到短信时，可以联系中国网建的客服（QQ联系即可，方便、快捷），修改验证码网关和绑定短信模板，短信模板中的变量用x进行代替，详情可以咨询中国网建的客服人员，这样就可以实现短信秒到用户手机中去；
非常注意：绑定了短信模板后，只有发送短信的内容与绑定的短信模板一模一样才能够实现短信的秒到，如果不一样的话，短信收到的时间将会变长；

4. 代码部分，首先要下载相应的jar包
* commons-codec-1.4.jar
* commons-httpclient-3.1.jar
* commons-logging-1.1.1.jar
* [下载](https://pan.baidu.com/s/1TCrZwWi9jd08E4ElHpz6qw)
提取码：r8f5 

代码可以直接套用模板，只需替换相关内容即可
```
import org.apache.commons.httpclient.Header;
import org.apache.commons.httpclient.HttpClient;
import org.apache.commons.httpclient.HttpException;
import org.apache.commons.httpclient.NameValuePair;
import org.apache.commons.httpclient.methods.PostMethod;

import java.io.IOException;

public class SendMsg_webchinese {
    public static void main(String[] args) throws HttpException, IOException {
        HttpClient client = new HttpClient();
        PostMethod post = new PostMethod("http://gbk.sms.webchinese.cn");
        // PostMethod post = new PostMethod("http://sms.webchinese.cn/web_api/");
        post.addRequestHeader("Content-Type",
                "application/x-www-form-urlencoded;charset=gbk");// 在头文件中设置转码
        NameValuePair[] data = {new NameValuePair("Uid", "注册的用户名"),// 注册的用户名
                new NameValuePair("Key", "密钥"),// 注册成功后，登录网站后得到的密钥
                new NameValuePair("smsMob", "手机号码"),// 手机号码
                new NameValuePair("smsText", "短信内容")};// 短信内容
        post.setRequestBody(data);

        client.executeMethod(post);
        Header[] headers = post.getResponseHeaders();
        int statusCode = post.getStatusCode();
        System.out.println("statusCode:" + statusCode);
        for (Header h : headers) {
            System.out.println("---" + h.toString());
        }
        String result = new String(post.getResponseBodyAsString().getBytes(
                "gbk"));
        System.out.println(result);
    }
}
```