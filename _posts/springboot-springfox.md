---
title: SpringBoot Springfox
date: 2016-10-29 10:57:37
categories: 后端
tags: 
  - springboot
  - idea
  - gradle
  - springfox
---

## Springfox

`Automated JSON API documentation for API's built with Spring`

自动生成JSON-API文档通过Spring项目

相信大多数朋友都遇到过这样一个场景：明明调用的是之前约定好的API，拿到的结果却不是想要的。可能因为是有人修改了API的接口，却忘了更新文档；或者是文档更新的不及时；又或者是文档写的有歧义，大家的理解各不相同。总之，让API文档总是与API定义同步更新，是一件非常有价值的事。

> [https://springfox.github.io/springfox/](https://springfox.github.io/springfox/)

## 文档

官方教程：https://springfox.github.io/springfox/docs/current/
官方API：https://springfox.github.io/springfox/javadoc/current/

<!--more-->

## 使用教程

### 1. 新建 `SpringBoot Gradle` 工程

<img src="struct.png"/>

### 2. 在 `build.gradle` 中添加依赖

```gradle
dependencies {
	compile('org.springframework.boot:spring-boot-starter-web')
	compile('io.springfox:springfox-swagger2:2.6.0')
	compile('io.springfox:springfox-swagger-ui:2.6.0')
	testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

### 3. 新建 `SwaggerConfig` 类

`@EnableSwagger2`、`@Configuration`

```java
@EnableSwagger2
@Configuration
public class SwaggerConfig {

    @Bean
    public Docket testApi() {
        return new Docket(DocumentationType.SWAGGER_2);

    }
}
```

> 经过上面3步后启动服务，访问：http://localhost:8080/swagger-ui.html 就完成了集成。

<img src="success1.png"/>

### 4. 创建controller测试
```java
@Api(tags = {"user"}, description = "user控制层描述")
@RequestMapping("user")
@Controller
public class UserController {

    @ApiOperation(value = "入参字符串", notes = "传入一个字符串")
    @ApiImplicitParam(name = "name", value = "用户姓名", required = true, dataType = "string", paramType = "query")
    @RequestMapping(value = "testStr", method = RequestMethod.GET)
    @ResponseBody
    public Map<String, Object> testStr(String name) {
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("name", name);
        return map;
    }
}
```

- `@Api` 类描述-------------------下图中1、2
- `@ApiOperation` 方法描述--------下图中3、5
- `@ApiImplicitParam` 参数描述----下图中6
- `method` 请求方法类型描述--------下图中4

> 更多注解：https://github.com/swagger-api/swagger-core/wiki/Annotations-1.5.X

### 结果

<img src="annotation.png"/>


## 实战介绍

**1. 配置api信息**

编写类 `SwaggerConfig`
```java
	@Bean
    public Docket testApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(myApiInfo());

    }

    /**
     * api信息描述
     *
     * @return
     */
    private ApiInfo myApiInfo() {
        ApiInfo apiInfo = new ApiInfo("Springfox",                      //大标题
                "REST API",                                             //小标题
                "0.1",                                                  //版本
                "NO terms of service",
                new Contact("xujiuhua", "url", "xujiuhuamoney@163.com"),//作者
                "The Apache License, Version 2.0",                      //链接显示文字
                "http://johuer.coding.me"                               //网站链接
        );

        return apiInfo;
    }
```

<img src="ui.png"/>

**2. 请求参数为对象实体**

如下User
```java
 	@ApiOperation(value = "入参对象", notes = "需要传入obj对象")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "name", value = "姓名", required = true, dataType = "string", paramType = "query"),
            @ApiImplicitParam(name = "sex", value = "性别(0-男,1-女)", required = true, dataType = "string", paramType = "query")
    })
    @RequestMapping(value = "testObj", method = RequestMethod.POST)
    @ResponseBody
    public Map<String, Object> testObj(User user) {
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("user", user);
        return map;
    }
```
如果对象有许多属性，不可能每个属性都一一描述，可以在实体类中增加以下注释，如
```java
public class User implements Serializable{

    @ApiModelProperty(value = "姓名")
    private String name;
    @ApiModelProperty(value = "年龄")
    private int age;
    @ApiModelProperty(value = "性别")
    private String sex ;

	// getter..  setter...
}
```

<img src="attribute.png"/>

**3. @ApiIgnore**

Swagger2默认将所有的Controller中的RequestMapping方法都会暴露，然而在实际开发中，我们并不一定需要把所有API都提现在文档中查看，这种情况下，使用注解@ApiIgnore来解决，如果应用在Controller范围上，则当前Controller中的所有方法都会被忽略，如果应用在方法上，则对应用的方法忽略暴露API。

**4. 源码**

[https://github.com/xujiuhua/SpringBootSwagger](https://github.com/xujiuhua/SpringBootSwagger)