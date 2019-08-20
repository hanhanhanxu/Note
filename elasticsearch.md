**elasticsearch不能用root用户启动，会报错。**



centos7的命令

```shell
id leyou # 查看leyou账户的权限
uid=1001(leyou) gid=1001(leyou) groups=1001(leyou)

su - leyou # 切换到leyou账户，不需要密码
su - root # 切换到root账户，需要密码
```



端口

elasticsearch：9200

9300端口底层采用另一种连接方式，tcp，性能比http好一些，集群间通信使用9300。但通过浏览器访问，只能使用9200端口，因为浏览器用的是http协议。

kibana：5601



每次打开虚拟机，都要手动启动elasticsearch。去elasticsearch目录下的`/home/leyou/elasticsearch/bin/`，运行`./elasticsearch`。



运行kibana，去kibana目录，bin下，双击运行kibana.bat