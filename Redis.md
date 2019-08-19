# 介绍

NOSql（非关系型数据库的统称/泛指） = NOT ONLY SQL不仅仅是SQL

代表：redis  memcached 都是key-value的NoSQL



Redis是C语言编写的，高性能的开源的NoSQL，数据保存在内存中，不定期持久化，保证数据安全。

Redis是以key-value形式储存，非关系型数据库严格来说不是一种数据库，应该**是一种数据结构化储存方法的集合**。

数据结构：组织数据的方式和提供的api



Memcached和Redis都是key-value NoSQL，他们储存数据都支持内存，读写性能高，并且都支持存储过期。

Redis相较于Memcached支持value类型更多，并且还支持持久化。



redis放入的不是set, hash, list等，而是一个一个的string，只是底层把他组织成了特定数据类型。

## jedis

> 客户端工具：jedis

redis和jedis的区别：

redis是服务端，jedis是客户端，使用jedis这个java客户端操作redis数据库。



**jedis连接池的方式操作redis。**

![1562894926566](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1562894926566.png)



# 开启

> 开启服务端：redis-server.exe redis.windows.conf

redis.windows.conf是redis的配置文件,~~不写也是默认加载此配置文件~~

> 开启客户端：redis-cli.exe -h 127.0.0.1 -p 6379 -a liyalong

或者 

> redis-cli.exe # 此时打开了默认连接和端口（conf配置文件可以配置，但是并没有输入密码，所以没有权限操作redis）
>
> auth password

# 查看

查看所有配置信息

config get *

查看指定配置(bind)信息

config get bind



# 数据类型

> string(字符串) , hash(哈希) , list(列表) , set(集合) , zset(有序集合)

## String

字符串，二进制安全，redis的string可以包含任何数据：jpg图片或者序列化对象。

最大能储存512MB

### 实例

> set name hanxu # 加不加双引号都行 "hanxu"
>
> OK
>
> get name
>
> "hanxu"

一个键最大能存储512MB

> del name # 用于删掉之前的key

https://www.runoob.com/redis/redis-strings.html

## Hash

键值对，string类型的field和value的映射表。

### 实例

> hmset person name hanxu age 18
>
> OK
>
> hget person name
>
> "hanxu"
>
> hget person age
>
> "18"

```
HMSET myhash field1 "Hello" field2 "World"
```

每个hash可以存储2^32-1键值对（42亿）

https://www.runoob.com/redis/redis-hashes.html

## List

字符串列表，有序。可以添加一个元素到列表的头部或尾部。

### 实例

> lpush name hanxu
>
> 1
>
> lpush name ctj
>
> 2
>
> lpush name hanye
>
> 3
>
> lrange name 0 5
>
> "hanxu"
>
> "ctj"
>
> "hanye"

每个list列表最多可存储2^32-1元素（42亿）

https://www.runoob.com/redis/redis-lists.html



list常用操作：

```sql
lpush tels huawei vivo oppo # 从左边插入数据
3
lrange tels 0 -1 # 从左边依次输出数据
1) "oppo"
2) "vivo"
3) "huawei"
rpush tels apple # 从右边加入数据
4
lrange tels 0 -1 
1) "oppo"
2) "vivo"
3) "huawei"
4) "apple"
lpop tels # 从左边弹出数据
"oppo"
rpop tels # 从右边弹出数据
"apple"
# 添加一些数据
1) "zhongxing"
2) "vivo"
3) "huawei"
4) "xiaomi"
 lrem tels 1 zhongxing # 从左边删除1个和zhongxing同名的元素
(integer) 1
127.0.0.1:6379> lrange tels 0 -1
1) "vivo"
2) "huawei"
3) "xiaomi"
127.0.0.1:6379> lrem tels -1 huawei # 从左边删除-1位，就是从右边删除1个，与huawei同名的元素
(integer) 1
127.0.0.1:6379> lrange tels 0 -1
1) "vivo"
2) "xiaomi"
# 添加一些数据
1) "vivo"
2) "vivo"
3) "xiaomi"
4) "vivo"
127.0.0.1:6379> lrem tels 0 vivo # 如果参数是0的话，就是删除所有与vivo同名的元素
(integer) 3
127.0.0.1:6379> lrange tels 0 -1
1) "xiaomi"
```

> lindex key index # 返回列表key中，下标为index的元素
>
> ltrim key start stop # 将原列表进行剪切为新列表，前闭后开



**★★★★★** Redis的list的数据结构是**栈**，先进后出，~~不是队列~~



## set

string类型无序集合，通过哈希表实现，添加，删除，查找的复杂度都是O(1)

> del name
>
> sadd name hanxu
>
> 1
>
> sadd name ctj
>
> 1
>
> sadd name hanxu
>
> 0
>
> sadd name hanye
>
> 1
>
> smembers name
>
> "ctj"
>
> "hanxu"
>
> "hanye"

第二次插入key name , value hanxu 时，已经存在(name,hanxu)，所以插入失败返回0

集合中最大的成员数为2^32-1

https://www.runoob.com/redis/redis-sets.html

## zset

（sorted set）自动排序集合（≠有序集合），是set的加强体，增加了一个分数字段，添加元素时必须为该元素添加一个分数，输出集合时，按照分数从小到大输出，而非按照输入顺序输出。

sorted set的实现是skip list(双向链表)和hash table的混合体

> del name
>
> 1
>
> zadd name 2 ctj
>
> 1
>
> zadd name 0 hanxu
>
> 1
>
> zadd name 4 hanye
>
> 1
>
> zadd name 1 hansi
>
> 1
>
> zadd name -1 zhangsan
>
> 1
>
> zadd name 3 ccc
>
> 1
>
> zrangebyscore name -10 10   好像没什么用 就算是写 0 10 也还是会输出-1分数的
>
> 1) "zhangsan"
> 2) "hanxu"
> 3) "hansi"
> 4) "ctj"
> 5) "ccc"
> 6) "hanye"
>
> zadd name 5 hanxu
>
> 0
>
>  zrangebyscore name -10 10
> 1) "zhangsan"
> 2) "hansi"
> 3) "ctj"
> 4) "ccc"
> 5) "hanye"
> 6) "hanxu"

向zset中添加已存在元素，返回0，并更新此元素的score。

https://www.runoob.com/redis/redis-sorted-sets.html

# Redis keys 命令

del key # key存在时将其删除

keys * # 查看所有key

dump key # 序列化key，并返回被序列化的值

exists key # 指定key是否存在

expire key seconds # 为key设置过期时间，以秒计算。

ttl key # 返回key的剩余生存时间，以秒计算（Time To Live）。

move key db # 将当前数据库的key移动到给定的数据库db中。

persist key # 移除Key的过期时间，key将持久保持

randomkey # 从当前数据库随机返回一个Key , 返回nil表示没有key

rename key newkey # 修改key的名称

renamenx key newkey # 仅当newkey不存在时，将key改名为newkey

type key # 返回key所储存的值的类型



## 过期

expire key 30 # 给某key设置30s过期时间，**此key必须存在** ，成功返回1，失败返回0

ttl key # 查看某key还有多少时间过期

若给某key设置30s过期时间，则30s后get key 返回nil，此key过期后自动消失。



# HyperLogLog

> 版本>=2.8.9才能使用

用来做基数统计的算法

>pfadd name hanxu # 将指定元素添加到JHyperLogLog中
>
>1
>
>pfadd name ctj
>
>1
>
>pfadd name hanye
>
>1
>
>pfadd name hansi
>
>1
>
>pfcount name # 返回给定HyperLogLog的基数估算值
>
>4

# 发布订阅

**发布订阅者的名字  大小写敏感**

Redis发布订阅(pub/sub)是一种消息通信模式，Redis客户端可以订阅任意数量的频道。

![1562561141427](C:\Users\123\AppData\Roaming\Typora\typora-user-images\1562561141427.png)

开启一个客户端，redis-cli.exe -h 127.0.0.1 -p 6379 -a liyalong

> subscribe redisChat # 订阅名为redisChat的频道（没有频道的定义）

再开启一个客户端，redis-cli,exe -h 127.0.0.1 -p 6379 -a liyalong

> publish redisChat "name is hanxu" # 发布消息时，如果是一个单词可以不加双引号，如果是多个有空格分开的单词，必须加双引号
>
> 1

订阅者客户端会收到如下消息

>"message"
>
>"redisChat"
>
>"name is hanxu"

pubsub channels # 查看当前发布订阅状态

# Redis事务

> 应用：一次执行多个命令。
>
> 方式：开始事务--->命令入列--->执行事务
>
> 若有命令执行失败，其余命令依然被执行；
>
> 在事务执行过程中，不会收到其他客户端命令的干扰；
>
> redis的事务是原子性的，但是redis并没有维护任何能够维持此原子性的机制，所以redis事务的执行是非原子性的，若有失败的指令，则不会回滚之前的指令，也不会影响后面的指令执行。（可以理解成简单的批量打包的脚本命令）

multi # 标记一个事务块的开始

exec # 执行所有事务块内的命令

discard # 取消事务，放弃执行事务块内的所有命令

watch key # 监视一个或多个Key，如果在执行事务前key被改动，那么事务被打断

unwatch # 取消watch命令对所有key的监视

```xml
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set a hanxu
QUEUED
127.0.0.1:6379> set b ctj
QUEUED
127.0.0.1:6379> set c hanye
QUEUED
127.0.0.1:6379> set d hansi
QUEUED
127.0.0.1:6379> set e ccc
QUEUED
127.0.0.1:6379> set f ttt
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) OK
4) OK
5) OK
6) OK
```

multi 表示开启事务，exec表示执行事务，将已加入队列的事务全部执行。



# Redis脚本

> Redis脚本使用Lua解释器执行脚本，Redis2.6版本通过内嵌支持Lua环境，

基本和语法：

> eval script numkeys key [key ...] arg [arg ...]

例如：

```xml
eval "return {keys[1],keys[2],argv[1],argv[2]}" 2 key1 key2 first second

1)"key1"
2)"key2"
3)"first"
4)"second"
```

命令：

>script exists script [script ...] # 查看指定脚本是否已被保存在缓存中
>
>script flush # 从脚本缓存中移除所有脚本
>
>script kill # 杀死当前正在运行的Lua脚本
>
>script load script # 将脚本script添加到脚本缓存中，但并不立即执行此脚本

# Redis连接

auth password # 验证密码是否正确 **也可用作连接redis**

echo message # 打印字符串  **没有输入密码时不可用**  单个单词不必加双引号，有空格时必须加双引号

quit # 关闭当前连接

select index # 切换到指定的数据库  select 1 select 2 等等 表示使用1号库使用2号库

redis默认有16个库，名称分别为0，1，2，3，，，15



**注意：**

> 只有加载配置文件启动时，才需要验证密码。

>如果不加载配置文件启动redis，那么就不需要验证密码（无论配置文件里面是否设置了密码）。

> **启动后在终端使用config set requirepass命令修改的密码是临时密码，只会对当前启动的服务端有效，重启redis服务端后就失效了。**(无论加不加在配置文件，都不会对配置文件中的requirepass做修改)



# Redis命令

> 用于管理redis服务和查询相关信息

```
info # 显示所有信息

info server # 显示server信息

config set parameter value # 修改redis配置参数，无需重启

save # 同步保存数据到硬盘

role # 返回主从实例所属的角色

shutdown [nosave][save] # 异步保存数据到硬盘，并关闭服务器

slaveof host port # 将当前服务器转变为指定服务器的从属服务器(slave server)

dbsize # 返回当前数据库的key的数量

flushdb # 删除当前数据库的所有key

flushall # 删除所有数据库的所有key

monitor # 实时打印出Redis服务器接收到的命令，调试用

debug segfault # 让Redis服务崩溃

command count # 获取Redis命令总数 : 172个

time # 返回当前服务器时间
127.0.0.1:6379> time
1) "1562567922"
2) "301343"
```

更多命令：https://www.runoob.com/redis/redis-server.html



## 所有命令：https://redis.io/commands