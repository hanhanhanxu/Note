# Spring常用注解

> 说说你在web后端开发时常用的注解

## Controller层

```java
import org.springframework.web.bind.annotation.*;
```

- @RestController (@ResponseBody + @Controller)

- @RequestMapping("")
- @Autowired (注入)
- @PostMapping (请求方式相关)
- @DeleteMapping
- @PutMapping
- @GetMapping
- @RequestParam (接受请求参数)
- @PathVariable (/user/{id} 大括号接受参数)
- @Api (swagger相关 2.9.2)
- @ApiOperation
- @ApiImplicitParams
- @ApiImplicitParam

## Service层

- @Service

```java
import org.springframework.stereotype.Service;
```

- @Autowired
- @Transactional

## Dao层（mapper层）

- @Insert
- @Select
- @Delete
- @Update

```java
import org.apache.ibatis.annotations.Insert;
```

```java
import org.apache.ibatis.annotations.*;
```

- @Param (https://www.javaboy.org/2019/0723/mybatis-@param.html

```java
import org.apache.ibatis.annotations.Param;
```

## pojo类

- @Table(name = "tb_brand")

```java
import javax.persistence.Table;
```

- @Id

```java
import javax.persistence.Id;
```

- @KeySql(useGeneratedKeys = true)

```java
import tk.mybatis.mapper.annotation.KeySql;
```

## 工具类

- @Data (lombok)

- @AllArgsConstructor
- @NoArgsConstructor

```java
import lombok.*;
```

- @Slf4j  (此处使用lombok的抽象层，实现层应该是使用的spring-boot-starter-test提供的)

```java
import lombok.extern.slf4j.Slf4j;
```





# Spring全家桶注解

@Component：仅仅表明一个组件，最普通的组件，可以被注入到Spring中管理

@Repository：表明一个类是Dao。**仅仅加注解没用，还要配置注解所在包扫描路径才能使注解生效**

- 具有将数据库操作抛出的原生异常翻译转化为spring的持久层异常的功能。

@Service：业务层

@Controller：控制层，(Spring-MVC注解)

- 具有将请求进行转发，重定向的功能。 



@Autowired：装配bean，默认按类型装配

- 属于Spring。如果允许null，则要设置required属性为false @Autowired(required=false) 

@Resource：装配bean，默认按名称装配

- 属于J2EE，JSR-250标准的注释





@EnableCaching：缓存管理功能

- 属于Spring3.1  在一个配置类上（@Configuration）添加此注解会触发一个post processor,使用缓存的bean



@EnableDiscoveryClient：将自己注册到eureka注册中心上去。

- 属于springcloud

- ```java
  import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
  ```





## lombok

@Data
@Setter
@Getter
@Log4j
@AllArgsConstructor
@NoArgsConstructor
@EqualsAndHashCode
@NonNull
@Cleanup
@ToString
@RequiredArgsConstructor
@Value
@SneakyThrows

@Synchronized



@Data：生成无参构造方法；属性的setter/getter方法；equals()，hashCode()，toString()，canEqual()方法

@Value：有参构造方法。类会被编译成final的，只有get方法没有set方法。



