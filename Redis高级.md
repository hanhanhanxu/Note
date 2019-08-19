# Redis高级教程

Redis是一种基于**客户端-服务端**模型以及**请求/响应**协议的**TCP服务**。

## Redis备份与恢复

**备份：**

> save 备份当前数据库    当前！！！

将会在redis安装目录中（D:\Redis-x64-3.2.100）创建dump.rdb文件

**恢复：**

只需要将dump.rdb文件移动到redis安装目录并启动服务端即可。

获取redis安装目录可以用命令config get dir 	

**例如：**

假如当前select 0 选择的是0数据库

```xml
set a hanxu
1
get a
"hanxu"
save
OK
```

上面代码中在0数据库里保存了一个key-value 为 a-hanxu , 并将此数据库备份到redis安装目录下的dump.rdb

然后

```xml
flushdb
get a
(nil)
```

上面命令中将0数据库中全部的key删除，然后get一个key为a的value，返回为nil即为空



然后**关闭服务端**(客户端没有要求)，**再打开服务端**，然后在客户端界面

```xml
get a
"hanxu"
```

**在后台创建redis备份文件：**

>  bgsave

## Redis安全

> config get requirepass # 查看是否设置密码
>
> config set requirepass hanxu # 设置密码为hanxu  设置密码后会立即需要验证才能使用命令
>
> auth hanxu # 相当于连接上redis但没有输入密码的时候输入密码  如果已经输入了密码，再使用auth输入一个错误的密码，就会使当前验证断开，需要再次输入新的密码才能执行redis命令

## Redis性能测试

在redis安装目录下执行：redis-benchmark -n 10000 -q

而非redis客户端内部指令

redis-benchmark -h 127.0.0.1 -p 6379 -t set,lpush -n 10000 -q

以上实例中主机为 127.0.0.1，端口号为 6379，执行的命令为 set,lpush，请求数为 10000，通过 -q 参数让结果只显示每秒执行的请求数。

## Redis客户端连接

在客户端输入

config get maxclients # 得到最大连接数

可以在服务端启动时设置最大连接数：

redis-server --maxclients 99999

或者在客户端设置

config set maxclients 10000

## Redis管道

Redis是一种基于客户端-服务端模型以及请求/响应协议的TCP服务。所以一个请求通常会遵循以下步骤：

- 客户端向服务端发送一个查询请求，并通常以阻塞模式监听Socket，等待服务器响应。
- 服务端处理命令，并将结果返回给客户端。

而上述这种模式通常**执行效率不高**，因为**阻塞监听Socket**，这就意味着一个客户端**在上一个命令未得到服务端的结果之前**，是一直阻塞而**不能发送其他指令**的。

---

而管道技术则可以在服务端未响应时，支持客户端继续向服务端发送请求，并最终一次性读取所有服务端的响应。

**管道技术显著的提高了redis服务的性能**



# redis配置文件

### 临时备份文件 appendonly.aof

> redis.windows.conf配置文件中的 ：  appendonly yes

appendonly = yes 实时记录操作日志，生成aof文件：appendonly.aof

![1562899841513](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1562899841513.png)

**aof日志解析：**

*2 ：接下来的命令，由两个部分组成

$6 ：第一个命令有6个字符串

SELECT

$1 ：接下来的命令有1个字符串

0





###　rdb持久化策略

在配置文件(redis.windows.conf)的 save 关键字。满足一定的策略就会做rdb持久化

```xml
save 900 1 # 15min操作1条数据  （要到达15min才行）
save 300 10 # 5min操作10条数据
save 60 10000 # 1min操作1w条数据
```





**redis持久化原理分析：**

当我们往内存中写数据的时候，我们并不是立即做持久化，只有满足持久化策略时才做持久化；而在这个过程中，还来不及做持久化的，通过aof日志的方式记录下来；当掉电重启后就把rdb和aof做并集。得到所有数据。

- 为什么不立即做持久化？

  为了提高效率

- rdb也是存，aof也是存，为什么还要搞aof呢？

  rdb有一定数据结构的，操作起来效率较低；而aof是文本文件，操作时间基本可以忽略不记。



**面试题：**

**redis数据的保存原理：**

redis可以把数据保存到内存中，而且为了保证数据的安全，还支持磁盘存储。具体的做法是当满足持久化策略时，做rdb的持久化；而没有满足的中间过程，以aof日志的方式保存下来。当服务器重启后，会加载rdb和aof做一个并集。





为什么要做集群？

一台机器内存不足，一台redis不支持高并发。







**集成Spring的原理**（所有东西集成Spring的原理都一样）：

把核心对象交给Spring管理；如果有事务的话，将事务交给Spring统一管理。



配置对象，连接池对象，连接池



redis库满了怎么办？采用合适的淘汰策略淘汰掉一些数据。

allkeys-lru ：从数据集(server.db[i].dict)中挑选最近最少使用的数据淘汰



为什么要使用缓存？

- 减少数据库压力
- 提高访问效率

原因？

- 对数据库的访问少了
- 内存级别的访问效率比磁盘高



面试题：

**如何选择合适的缓存？**

非集群环境下可以使用jpa,mybatis的二级缓存，或者用比较厉害的redis的中央缓存。但是集群环境下只能用redis的中央缓存。







### 怎么存储数据？

- json方案：

存到redis中时，可以将对象转化为json格式字符串存入，取出时再将json格式字符串转化为对象。

Json框架：Jackson(SpringMvc中的)，gson(google的) fastjson(阿里的)

fastgson不需要依赖任何第三方jar包，速度比所有其他的json转换都块。





- 序列化方案：

将对象转化为二进制数组，将二进制数组转化为对象。

