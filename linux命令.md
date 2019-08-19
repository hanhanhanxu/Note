### 查看相关进程

ps -ef | grep nginx   ： **全格式显示所有与nginx相关的进程**

- ps 命令将某个进程显示出来
- grep 命令是查找，文本搜索工具，能使用正则表达式搜索文本
- 中间的 | 的管道命令，是指ps 命令与grep命令同时执行
- ps命令有一些参数： 
  -e : 显示所有进程 
  -f : 全格式 
  -h : 不显示标题 
  -l : 长格式 
  -w : 宽输出 
  a ：显示终端上的所有进程，包括其他用户的进程。 
  r ：只显示正在运行的进程。 
  u ：以用户为主的格式来显示程序状况。 
  x ：显示所有程序，不以终端机来区分。

ps -ef | grep java 全格式显示所有与java相关的进程。

ps -ef|grep 全格式显示所有进程



### 查看日志

tail -f log/catalina.out  **查看log/catalina.out中的最底部的日志(并不断随文件更新显示)**

tail 查看文件内容

- -f 循环读取

tail -f filename 把filename文件里最尾部的内容显示在屏幕上(并不断刷新，更新显示内容)。





nohup java -jar -Xms512m -Xmx512m sys-miniProgram-admin-1.5.0-SNAPSHOT.jar > log/catalina.out 2>&1 & 运行jar包

