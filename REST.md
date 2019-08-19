### rest风格下controller中返回结果

1、有值时（查询）：

```java
public ResponseEntity<PageResult<Brand>> queryBrandByPage(
```

```java
return ResponseEntity.ok(result);
```

2、无值时（新增）：

```java
public ResponseEntity<Void> addBrand(Brand brand, List<Long>cids){
```

```java
return ResponseEntity.status(HttpStatus.CREATED).build();
```

rest中，如果有返回体就选.body(T)

没有就选.build();



当传入的参数有多个，而你又要接受pojo又同时想要单独接收其中的某些

那么你就应该给单独接收的加上@RequestParam注解。





### Rest风格响应要求：

```java
/**
 * ResponseEntity
 *表示整个HTTP响应：状态代码，标题和正文。
 *
 */
```

> 增加post成功：返回 201  

```java
return ResponseEntity.status(HttpStatus.CREATED).build();
```

> 删除delete成功：返回204

```java
return ResponseEntity.status(HttpStatus.NO_CONTENT).build();
```

> 修改put成功：返回200（操作成功） 或 204（操作已成功执行，但无数据返回）

```java
return ResponseEntity.status(HttpStatus.OK).build();
```

> 查询get成功：返回200 

```java
return ResponseEntity.ok(list);
```



![1564119565028](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564119565028.png)



@ResponseBody ：将java对象序列化，放到Body（响应体）里面去。不是序列化为json，只是我们默认序列化为json。

