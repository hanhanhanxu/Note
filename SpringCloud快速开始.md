# 1、创建工程

## 1.1、 创建父工程

### 1.1.1、 图示

![1564035256012](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564035256012.png)

![1564035278319](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564035278319.png)

![1564035295777](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564035295777.png)



### 1.1.2、 pom依赖添加

> 注意，父工程项目做了依赖版本管理，子工程就不再需要管理依赖版本。
>
> 并且SpringBoot parent父工程本来就做了许多依赖版本管理，所以子工程有许多依赖均不需要做版本设置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.parent</groupId>
    <artifactId>leyou</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <!-- 父工程打包方式: pom -->
    <packaging>pom</packaging>

    <!-- 项目描述 -->
    <name>leyou</name>
    <description>Demo project for Spring Boot</description>

    <!-- springboot父依赖 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <!-- 版本管理 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <spring-cloud.version>Finchley.RC1</spring-cloud.version>
        <mybatis.starter.version>1.3.2</mybatis.starter.version>
        <mapper.starter.version>2.0.3</mapper.starter.version>
        <druid.starter.version>1.1.9</druid.starter.version>
        <mysql.version>5.1.32</mysql.version>
        <pageHelper.starter.version>1.2.5</pageHelper.starter.version>
        <!-- 本项目的版本 -->
        <leyou.latest.version>1.0.0-SNAPSHOT</leyou.latest.version>
        <fastDFS.client.version>1.26.1-RELEASE</fastDFS.client.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- springCloud -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- mybatis启动器 -->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.starter.version}</version>
            </dependency>
            <!-- 通用Mapper启动器 -->
            <dependency>
                <groupId>tk.mybatis</groupId>
                <artifactId>mapper-spring-boot-starter</artifactId>
                <version>${mapper.starter.version}</version>
            </dependency>
            <!-- 分页助手启动器 -->
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>${pageHelper.starter.version}</version>
            </dependency>
            <!-- mysql驱动 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <!--FastDFS客户端-->
            <dependency>
                <groupId>com.github.tobato</groupId>
                <artifactId>fastdfs-client</artifactId>
                <version>${fastDFS.client.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <!-- 工具类 -->
    <dependencies><!-- dependencies中的引用，所有子工程都要依赖 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.4</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

</project>
```



## 1.2、 创建子工程-注册中心

> 选中leyou，右键，添加Module

### 1.2.1、 图示

![1564036120784](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564036120784.png)

![1564036127737](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564036127737.png)

![1564036136536](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564036136536.png)

### 1.2.2、 pom依赖添加

> 主要添加eureka注册中心依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou.parent</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.parent</groupId>
    <artifactId>ly-registry</artifactId>

    <dependencies>
        <!-- eureka注册中心依赖 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
    </dependencies>

</project>
```

### 1.2.3、 eureka实例

> 在当前模块下新建类com.leyou.LyRegistry.java类，配置并开启eureka服务。

```java
package com.leyou;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class LyRegistry {
    public static void main(String[] args) {
        SpringApplication.run(LyRegistry.class);
    }
}

```

### 1.2.3、 application.yml文件配置

> 主要配置eureka.client.service-url。它是一个Map键值对集合。

```yml
server:
  port: 10086
spring:
  application:
    name: ly-registry
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

## 1.3、 创建子工程-网关

> 选中leyou，右键，添加Module

### 1.3.1、 图示

![1564037095820](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564037095820.png)

![1564037103657](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564037103657.png)

![1564037109672](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564037109672.png)

### 1.3.2、 pom依赖添加

> 主要添加zuul网关依赖，eureka-client注册中心客户端依赖。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou.parent</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.common</groupId>
    <artifactId>ly-gateway</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>

</project>
```

### 1.3.3、 zuul实例

> 在当前模块下新建com.leyou.gateway.LyGateway.java类，SpringCloudApplication包含三个注解

```java
package com.leyou.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.cloud.client.SpringCloudApplication;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;

/*
@SpringBootApplication
@EnableDiscoveryClient  注册到eureka
@EnableCircuitBreaker  熔断器
 */
@EnableZuulProxy  //zuul
@SpringCloudApplication
public class LyGateway {
    public static void main(String[] args) {
        SpringApplication.run(LyGateway.class);
    }
}

```

 ### 1.3.4、 appilcation.yml

> 主要配置服务名，eureka注册实例，熔断，负载均衡

```yml
server:
  port: 10010
spring:
  application:
    name: api-gateway
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
zuul:
  prefix: /api # 添加路由前缀
hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMillisecond: 5000 # 熔断超时时长：5000ms
ribbon:
  ConnectTimeout: 1000 # 连接超时时间(ms)
  ReadTimeout: 3500 # 通信超时时间(ms)  （ConnectTimeout+ReadTimeout）*2 不能超过 熔断超时时长  5000ms
  MaxAutoRetries: 0 # 当前服务重试次数
  MaxAutoRetriesNextServer: 0 # 切换服务重试次数

```

## 1.4、 创建子工程-业务模块

> 我们将一个业务服务，分为两大部分：实体类服务，业务服务。

### 1.4.1 、 图示

> 选中leyou，右键，添加Module

![1564040074167](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040074167.png)

![1564040118140](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040118140.png)

![1564040130171](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040130171.png)

### 1.4.2、 pom修改

> 一个业务微服务分为两个微服务，所以这个业务微服务是个聚合工程，要修改打包方式为pom。
>
> <packaging>pom</packaging>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>leyou</artifactId>
        <groupId>com.leyou.parent</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.service</groupId>
    <artifactId>ly-item</artifactId>
    <!-- 修改打包方式为pom类型。ly-item：聚合工程。 -->
    <packaging>pom</packaging>


</project>
```

### 1.4.3、 创建实体类微服务工程

> 选中ly-item，右键，添加Module

#### 1.4.3.1、 图示

> 实体类微服务只是存放实体类，不涉及任何业务。所以pom中不需引入任何依赖。

![1564040539269](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040539269.png)

![1564040552134](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040552134.png)

![1564040565668](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040565668.png)

### 1.4.4、 创建业务微服务工程

> 选中ly-item，右键，添加Module

#### 1.4.4.1、 图示

![1564040627333](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040627333.png)

![1564040640308](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040640308.png)

![1564040658875](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564040658875.png)

#### 1.4.4.2、 pom依赖添加

> 主要添加业务依赖，和实体类依赖。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>ly-item</artifactId>
        <groupId>com.leyou.service</groupId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.leyou.service</groupId>
    <artifactId>ly-item-service</artifactId>

    <dependencies>
        <!-- 业务相关依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!-- 通用mapper包含了jdbc和mybatis依赖 -->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- 实体类依赖 -->
        <dependency>
            <groupId>com.leyou.service</groupId>
            <artifactId>ly-item-interface</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

</project>
```

#### 1.4.4.3、 业务实例

> 在ly-item-service模块下新建com.leyou.LyitemApplication.java类，
>
> 配置@EnableDiscoveryClient将自己注册到eureka注册中心。

```java
package com.leyou;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class LyitemApplication {
    public static void main(String[] args) {
        SpringApplication.run(LyitemApplication.class);
    }
}
```

#### 1.4.4.4、 application.yml

> 主要配置：端口，服务名称，数据源，eureka地址（集群下配置多个eureka，逗号, 分隔）。

```yml
server:
  port: 8081
spring:
  application:
    name: item-service
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/heima
    username: root
    password: hanxu
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

# 2、 添加路由规则

> 上面的配置搭建完成后，还要将业务路由规则配置到网关中，网关拦截相应请求，发送到指定服务。

在leyou模块下、ly-gateway模块下、resources目录下的 application.yml中添加路由规则：

```yml
zuul:
  prefix: /api # 添加路由前缀
  routes: # 添加路由规则
    item-service: /item/**
```

添加了一条routes：item-service: /item/**

拦截/item/**请求，发送给item-service服务。

# 3、 项目结构

![1564044029120](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564044029120.png)

# 4、 注意

## eureka中配置不注册自己。

```yml
eureka:
 client:
  register-with-eureka: false
  fetch-registry: false
```

## 网关配置拉去时长。fetch

网关默认拉取服务是30s一拉，可以配置拉去时长。fetch

> 网关的配置文件

```yml
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
    registry-fetch-interval-seconds: 30 
```

# 5、 添加通用工具类服务

> 大型项目需要通用的工具类，单独作为一个微服务

> 选中leyou，右键，添加Module

## 5.1、 图示

![1564046058769](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564046058769.png)

![1564046092664](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564046092664.png)

![1564046108584](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1564046108584.png)

