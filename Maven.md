# Maven

> maven中央库：http://repo1.maven.org/maven2/
>
> mvnrepository仓库（查找maven依赖）：https://mvnrepository.com/
>
> apache maven官方教程：http://maven.apache.org/guides/getting-started/index.html
>
> 可搜索的maven中央库：https://search.maven.org/

## Maven的几个库

- **1、Maven本地资源库**

  > Maven本地资源库是在自己电脑上的，用来存储之前使用maven时下载的jar包文件的库。
  >
  > 就是说当你使用maven构建项目时，如果引入了依赖，maven第一次会从远程库（网络上）下载，然后在之后使用maven构建项目时，如果使用到了之前引入过的依赖，那么maven将会从本地资源库引入jar包依赖，而不是再去远程（网络上）下载。
  >
  > 简单来说，当你使用maven构建一个项目，所有相关文件将被存储在你的Maven本地资源库中

  ```xml
  默认情况下，Maven的本地资源库默认为.m2文件夹：
  Unix/Mac OS X - ~ /.m2
  Windows-C:\Documents and Settings\{your-username}\.m2
  ```

  当然你也可以更改Maven本地库：

  > 修改{M2_HOME}\conf\setting.xml配置文件（Maven核心配置文件）
  >
  > 查找localRepository关键字：
  >
  > ```xml
  > <settings><!-- localRepository
  >    | The path to the local repository maven will use to store artifacts.
  >    |
  >    | Default: ~/.m2/repository
  >   <localRepository>/path/to/local/repo</localRepository>
  >   -->
  > <localRepository>D:\apache-maven-repository</localRepository>
  > ```
  >
  > 这里我将我们Maven本地库修改为D:\apache-maven-repository(需要先建立D:\apache-maven-repository文件夹)，以后我们的jar包等文件都会下载到本地仓库D:\apache-maven-repository

- **2、Maven中央存储库**

  当你使用maven构建一个项目时，Maven会检查你的pom.xml文件，确定需要哪些依赖，然后会先从你的本地库中去找是否有，有就直接引入；没有就会从**Maven中央储存库**查找下载(http://repo1.maven.org/maven2/) 可能当你看时这个网站已经关闭了浏览功能，但是当你构建项目时她依然会从此网站下载jar包等文件（你可以从Maven的输出中验证）。目前你可以从这个https://search.maven.org/ 网站查看Maven中央存储库。

- **3、Maven远程存储库**

  > 若你使用Maven构建一个项目，当有一个依赖，既不能从Maven本地库中找有，也不能从Maven中央库中找到时，Maven就会报错。

  原因是有的依赖只存在于特定的仓库，不被Maven中心仓库包含，比如某些特定的组织或个人编写的jar包等资源，或者种种原因还尚未被Maven中心库包含。

  比如org.jvnet.localizer 只适用于 [Java.net资源库](https://maven.java.net/content/repositories/public/) 这种情况下即使你正确的添加了依赖

  ```xml
  <dependency>
          <groupId>org.jvnet.localizer</groupId>
          <artifactId>localizer</artifactId>
          <version>1.8</version>
  </dependency>
  ```

  Maven也不能找到此依赖，因为**默认情况下Maven只会从本地库和中央库中去查找依赖**，而此依赖两者都不属于，**她属于另一个网站的资源**。所以就会报错找不到依赖。

  此时你应该**额外声明一个远程存储库**，这个远程库就是java,net的资源库，Maven允许我们在pom.xml文件中生命远程库。

  > 在Maven的pom.xml中添加repository节点
  >
  > ```xml
  > <repositories>
  > 	<repository>
  > 	    <id>java.net</id>
  > 	    <url>https://maven.java.net/content/repositories/public/</url>
  > 	</repository>
  > </repositories>
  > ```

---

**★综上所述**，如果你使用Maven构建项目，pom.xml文件中引入了一个依赖，那么查找此依赖的顺序为：

- 1、从本次仓库中查找({M2_HOME}/conf/setting.xml中配置的localRepository路径)
- 2、从Maven中央库中查找http://repo1.maven.org/maven2/ ， 你可以从https://search.maven.org/ 中查找（搜索） （前面是给Maven用的，后面是给人看的，给人搜索看到底有没有某个依赖。）
- 3、从pom.xml中repository节点配置的url中查找

如果以上都没有找到，则报错。



### 番外篇-远程库

由于某些原因，有些依赖不在Maven中央库中，只有从Java.net或JBoss的远程储存库中能找到。

- 1、Java.net远程库

  ```xml
  <repositories>
      <repository>
        <id>java.net</id>
        <url>https://maven.java.net/content/repositories/public/</url>
      </repository>
   </repositories>
  ```

  (旧的 “http://download.java.net/maven/2” )

- 2、JBoss远程库

  ```xml
  <repositories>
        <repository>
  		<id>JBoss repository</id>
  		<url>http://repository.jboss.org/nexus/content/groups/public/</url>
        </repository>
  </repositories>
  ```

  (旧的 *http://repository.jboss.com/maven2/*)

  当然也有很多公司或个人自己搭建一个Maven远程库，此情况下也可以使用repository节点引入你的远程库，比如：

  ```xml
  <repositories>
          <repository>
              <id>public</id>
              <name>do1 nexus</name>
              <url>http://dqdp.do1.com.cn/mvnrepository/content/groups/public/</url>
              <releases>
                  <enabled>true</enabled>
              </releases>
          </repository>
  </repositories>
  ```



### 番外篇-不存在于任何库中的jar

两种情况：

-  1、要使用的jar不存在于Maven中央库和远程库中
- 2、自己写了一个jar，其他项目要用

这两种情况我们需要**定制库到Maven资源库**，这种jar(库)叫做“非Maven支持”库

方法：

先找到jar资源，利用mvn install命令将jar安装（下载）到本地，然后在pom.xml中引入依赖。

（**注意：这种情况下如果我们的项目拷贝到其他电脑上，那么pom.xml会报错的**，因为其他电脑上没有这个jar文件，也无法从网络上下载下来。此时需要手动mvn install安装此jar）



**具体方法参考链接**：https://www.yiibai.com/maven/include-library-manully-into-maven-local-repository.html





## Maven生命周期

![1562642287359](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1562642287359.png)



| 阶段          | 处理     | 描述                                                     |
| :------------ | :------- | :------------------------------------------------------- |
| 验证 validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的                   |
| 编译 compile  | 执行编译 | 源代码编译在此阶段完成                                   |
| 测试 Test     | 测试     | 使用适当的单元测试框架（例如JUnit）运行测试。            |
| 包装 package  | 打包     | 创建JAR/WAR包如在 pom.xml 中定义提及的包                 |
| 检查 verify   | 检查     | 对集成测试的结果进行检查，以保证质量达标                 |
| 安装 install  | 安装     | 安装打包的项目到本地仓库，以供其他项目使用               |
| 部署 deploy   | 部署     | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |



Maven 有以下三个标准的生命周期：

- **clean**：项目清理的处理
- **default(或 build)**：项目部署的处理
- **site**：项目站点文档创建的处理



https://www.runoob.com/maven/maven-build-life-cycle.html



## Maven构建命令

> https://www.cnblogs.com/xdp-gacl/p/4240930.html

```xml
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

或者

mvn archetype:create -DgroupId=com.mycompany.app -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false


 mvn archetype:generate -DgroupId=hx.insist -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```





## Maven的依赖范围Scope

不写<scope>标签，默认是<scope>compile</scope>



- **compile**：项目整个生命过程都有用，在打包时会一同发布。Maven中Scope属性的默认值。此依赖会传递给所有依赖于当前项目的子项目。
- **runtime**：运行和测试有效，编译时不生效
- provided：已提供范围。打包的时候不用加进去，其他设施会提供。该依赖相当于compile参加所有过程，但是在打包阶段做了exclude
- system：类似于provided，不过是从本地文件系统拿，必须配合systemPath属性。
- **test**：仅在测试下生效，@Test注解的方法下有效
- import：Maven2.0.9版本后可用，只可以用在dependencyManagement中，用来解决Maven的单继承和公共继承的版本号问题。

## Maven的默认打包方式

### pom工程

应用：**父工程**或**聚合工程**中，用来做**jar包的版本控制**。必须指明这个聚合工程的打包方式为pom。

举例：

- ly-parent，整个项目的父工程，用来做版本控制，必须明确打包为pom
- ly-item，聚合工程。下面包括ly-item-interface（实体类）和ly-item-service（业务）两个服务。必须名且打包为pom

```xml
<packaging>pom</packaging>
```

### war工程

应用：发布在服务器上运行，如**网站**或**服务**。Spring Boot中，只要我们引入**web启动器依赖**，那么maven自动帮我们识别这个项目为war工程。

举例：

- ly-item下的ly-item-service服务，将来要放在服务器上，浏览器直接访问，打成war包

添加spring-boot-starter-web依赖，则打为war包

```xml
<packaging>war</packaging>
```

### jar工程

不指明的话，默认达成jar包。

举例：

- ly-common，存放公用工具类：异常处理类，utils，公用页面Vo等。不明确指定，则打包为jar

```xml
<packaging>jar</packaging>
```

​	







## Tips:

在引入依赖时，可以忽略版本<version>标签，Maven会自动依赖最新版本的依赖，当中央库有新版本时也会自动更新至新版本。

Maven依赖机制简介：http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html

