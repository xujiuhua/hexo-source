---
title: MyBatis 传递多个参数
date: 2016-12-18 21:30:23
categories: 后端
tags: 
  - MyBatis
---

一共介绍三种方式

### 第一种方式

> 用#{0}表示第一个参数、#{1}表示第二参数...以此类推

<!--more-->

#### service
```java
public User findByMutiParam(String name, String password) {
    return userDao.findByMutiParam(name, password);
}
```

#### dao
```java
User findByMutiParam(String name, String password);
```

#### xml
```xml
<select id="findByMutiParam" resultType="com.mybatis.entity.User">
    SELECT
      *
    FROM
      user
    WHERE
      name = #{0} and password = #{1}
</select>
```

### 第二种方式

> 参数封装map集合

#### service
```java
public User findByMap(String name, String password) {
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("name", name);
    map.put("password", password);
    return userDao.findByMap(map);
}
```

#### dao
```java
User findByMap(Map<String, Object> map);
```

#### xml
```xml
<select id="findByMap" resultType="com.mybatis.entity.User">
    SELECT
      *
    FROM
      user
    WHERE
      name = #{name} and password = #{password}
</select>
```

### 第三种方式

> 使用注解（推荐）参数清晰明了

#### service
```java
public User findByAnnotation(String name, String password) {
    return userDao.findByAnnotation(name, password);
}
```

#### dao
```java
User findByAnnotation(@Param("name") String name, @Param("password") String password);
```

#### xml
```xml
<select id="findByAnnotation" resultType="com.mybatis.entity.User">
    SELECT
     *
    FROM
      user
    WHERE
      name = #{name} and password = #{password}
</select>
```
