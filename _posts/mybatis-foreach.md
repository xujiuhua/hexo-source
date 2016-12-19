---
title: Mybatis Foreach
date: 2016-12-18 23:35:16
categories: 后端
tags: 
  - MyBatis
---

## foreach简介

foreach的主要用在构建in条件中，它可以在SQL语句中进行迭代一个集合。foreach元素的属性主要有 item，index，collection，open，separator，close，网上解释比较多，使用时注意`collection`,这个是关键，容易错误，下面分别介绍5种情况。

<!--more-->

## collection的属性确定

### list

> 传入的是单参数且参数类型是一个List的时候，collection属性值为list

```java
// test
@Test
public void testFindByIdList() throws Exception {
    List<Integer> list = new ArrayList<Integer>();
    list.add(1);
    list.add(2);
    List<User> users = userService.findByIdList(list);
    assertEquals(2, users.size());
}

// service
public List<User> findByIdList(List<Integer> list) {
    return userDao.findByIdList(list);
}

// dao
List<User> findByIdList(List<Integer> list);
```

```xml
<select id="findByIdList" resultType="com.mybatis.entity.User">
    SELECT * FROM user WHERE id IN
    <foreach collection="list" index="index" item="item" open="(" separator="," close=")">
        #{item}
    </foreach>
</select>
```
### 数组

> 传入的是单参数且参数类型是一个array数组的时候，collection的属性值为array

```java
// test
@Test
public void testFindByIdArray() throws Exception {
    Integer[] ids = new Integer[] {1, 2};
    List<User> users = userService.findByIdArray(ids);
    assertEquals(2, users.size());

}

// service
public List<User> findByIdArray(Integer[] ids) {
    return userDao.findByIdArray(ids);
}

// dao
List<User> findByIdArray(Integer[] ids);
```

```xml
<select id="findByIdArray" resultType="com.mybatis.entity.User">
    SELECT * FROM user WHERE id IN
    <foreach collection="array" index="index" item="item" open="(" separator="," close=")">
        #{item}
    </foreach>
</select>
```

### set

> 传入的是单参数且参数类型是一个array数组的时候，collection的属性值为collection

```java
// test
@Test
public void findByIdSet() throws Exception {
    Set<Integer> set = new HashSet<Integer>();
    set.add(1);
    set.add(2);
    List<User> users = userService.findByIdSet(set);
    assertEquals(2, users.size());
}

// service
public List<User> findByIdSet(Set<Integer> set) {
    return userDao.findByIdSet(set);
}

// dao
List<User> findByIdSet(Set<Integer> set);
```

```xml
<select id="findByIdSet" resultType="com.mybatis.entity.User">
        SELECT * FROM user WHERE id IN
        <foreach collection="collection" index="index" item="item" open="(" separator="," close=")">
            #{item}
        </foreach>
    </select>
```


### map

> 传入的参数是多个的时候，封装成一个Map了，collection的属性值为map的key

```java
// test
@Test
public void testFindByMapForeach() throws Exception {
    final List<Integer> ids = new ArrayList<Integer>();
    ids.add(1);
    ids.add(2);
    Map<String, Object> map = new HashMap<String, Object>();
    map.put("ids", ids);
    map.put("name", "xujiuhua");
    List<User> users = userService.findByMapForeach(map);
    assertEquals(1, users.size());
}

//service
public List<User> findByMapForeach(Map<String, Object> map) {
    return userDao.findByMapForeach(map);
}

//dao
List<User> findByMapForeach(Map<String, Object> map);
```

```xml
<select id="findByMapForeach" resultType="com.mybatis.entity.User">
	SELECT * FROM user WHERE name = #{name} AND id IN
	<foreach collection="ids" index="index" item="item" open="(" separator="," close=")">
	    #{item}
	</foreach>
</select>
```

### `@Param`

> 传入的参数使用的`@Param`注解，collection的属性值为注解的名称

```java
// test
@Test
public void findByIdListAnnotation() throws Exception {
    List<Integer> list = new ArrayList<Integer>();
    list.add(1);
    list.add(2);
    List<User> users = userService.findByIdListAnnotation(list);
    assertEquals(2, users.size());
}

// service
public List<User> findByIdListAnnotation(List<Integer> list) {
    return userDao.findByIdListAnnotation(list);
}

// dao
List<User> findByIdListAnnotation(@Param("annotatedList") List<Integer> list);
```

```xml
<select id="findByIdListAnnotation" resultType="com.mybatis.entity.User">
    SELECT * FROM user WHERE id IN
    <foreach collection="annotatedList" index="index" item="item" open="(" separator="," close=")">
        #{item}
    </foreach>
</select>
```

## 总结

其实List和数组，已经内部封装为map集合了，相当于
- 单参数List的key为默认"list"，及map.put("list", list)，list的collections="list"
- 数组的key为默认"array"，及map.put("array", 数组)，数组的collections="array"
- map有自己指定key,map的collections="map的key"
- 注解是自己指定一个别名，所以key值也发生改变，注解的collections="别名"

> 内部全都是map的key，这样就不会出错了！！！


