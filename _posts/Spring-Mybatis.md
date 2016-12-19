---
title: Spring-Mybatis-Json
date: 2016-09-26 15:03:52
tags: 
 - Spring
 - Mybatis
 - JSON
---

## Spring-Mybatis-JSON

Spring版本是4.3.2，在用@ResponseBody标注返回json格式时候遇到这样的错误：“The resource identified by this request is only capable of generating responses with characteristics not acceptable according to the request "accept" headers.”。

解决办法：
[参考](http://www.tuicool.com/articles/Nvm2Y3e)

<!--more-->

#### gradle require
 ```gradle
'com.fasterxml.jackson.core:jackson-core:2.8.2',
'com.fasterxml.jackson.core:jackson-databind:2.8.2',
'com.fasterxml.jackson.core:jackson-annotations:2.8.2',
'org.codehaus.jackson:jackson-core-asl:1.9.13',
'org.codehaus.jackson:jackson-mapper-asl:1.9.13'
```

#### xml config

> note: `<mvc:annotation-driven />` must be after 

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
    </property>
</bean>

<mvc:annotation-driven />
```

