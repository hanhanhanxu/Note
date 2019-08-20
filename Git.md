#### 设置（重置）当前电脑上的git账户和邮箱

> global 全局

```
git config --global user.name "hanxu"
git config --global user.email "hanxu@do1.com.cn"
```

#### 生成公钥

```
ssh-keygen -t rsa -C "hanxu@do1.com.cn"
```

如果之前生成过ssh，那么这一步会出现提问：**是否覆盖之前的ssh?**

选择 y

生成的公钥文件在：C:\Users\123\\.ssh\id_rsa.pub 

#### 查看当前电脑上的git账户和邮箱

```
git config user.name
git config user.email
```

#### 重置电脑git账户信息 -unsure

```
git config –-system –-unset credential.helper
```

#### 记住git账户密码 -unsure

> 每次git clone都会让你输入账号和密码，执行：

```
git config --global credential.helper store
```







