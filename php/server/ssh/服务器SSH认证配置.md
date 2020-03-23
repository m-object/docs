现在使用 Apache 的 PHP 开发者越来越少了;大部分程序员都是使用 Niginx 服务器


## 设置服务器

购买服务器之后使用：SSH 登陆

```
ssh root@主机ip
```

> ssh 登陆，需要开放 22 端口(prot)


## 升级软件

升级操作系统中的软件：

##### Ubuntu

```
apt-get update
```

##### CentOS
```
yum update
```

更新系统能保证操作系统中默认的软件安装了最新的更新和安全修补

## 非根用户

现在你的新服务器还不安全。下面是一些能加强新服务器安全性的良好实践。

我们要创建非根用户。以后我们都应该使用非根用户登录服务器。根用户在服务器拥有无限权利，它是神，毫无疑问，它能执行任何命令。我们应该尽量让别人不能使用根用户访问服务器。

##### Ubuntu

创建一个名为 `deploy` 的非根用户。见到提示时，输入用户密码，然后按照屏幕上显示的说明做。

```
adduser deploy
```
接下来，执行下述命令，把 `deploy` 用户加入 `sudo` 用户组：

```
usermod -a -G sudo deploy
```

这么做是为了让 `deploy` 用户拥有 `sudo` 权限（即通过密码认证后可以执行需要特殊权限的任务）。

##### CentOS

创建一个名为 `deploy` 的非根用户：

```
adduser deploy
```

为新建的 `deploy` 用户设置密码：

```
passwd deploy
```
把新建的用户加入 `wheel` 用户组：

```
user -a -G wheel deploy
usermod -G wheel deploy // centos6
```

这么做是为了让 `deploy` 用户拥有 `sudo` 权限（即通过密码认证后可以执行需要特殊权限的任务）。


## SSH 密钥对认证

```
ssh deploy@123.456.789
```

使用 SSH 方法进行登陆服务器；但是我们可以禁用密码认证，加强安全。使用 SSH 登陆服务器时应该使用 SSH 密钥对认证。

使用 SSH 密钥对认证方式登陆远程服务器时，远程设备会随机创建一个消息，使用公钥加密，然后把密文发给本地设备。本地设备收到密文后使用私钥解密，然后把解密后的消息发给远程服务器。远程服务器验证解密后的消息之后，再赋予你访问服务器的权限。

如果要在多台电脑中登陆远程服务，或许不应该使用 SSH 密钥对认证。因为这样需要在每台本地电脑生成 SSH 密钥对，然后在把每个密钥对中的公钥复制到远程服务器中。遇到这种情况，或许最好使用最安全的密码进行密码认证。


创建 SSH 密钥对：

```
ssh-keygen
```

私钥应该保存在本地电脑中，而且要保密。公钥复制到服务器中，SCP（安全复制）：

```
scp ~/.ssh/id_rsa.pub deploy@123.456.789:   
```
> 注意一定要在末尾加上 ":",这个命令会把公钥复制到远程服务器中 deploy 用户的家目录里

查看 `~/.ssh` 目录是否存在：

```
cd ~/.ssh
```

不存在则创建：

```
mkdir ~/.ssh
```

然后执行以下命令,创建 `~/.ssh/authorized_keys` 「密钥授权」文件

```
touch ~/.ssh/authorized_keys
```

这个文件的内容是一系列允许登陆这台服务器的公钥。

执行下述命令，把刚上传的公钥添加到 `~/.ssh/authorized_keys` 文件中

```
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
```

最后我们要修改几个目录和文件的访问权限,只让 `deploy` 用户访问 `~/.ssh` 目录和 `~/,ssh/authorized_keys` 文件。这个目录和文件的访问权限由下述命令设置：
```
chown -R deploy:deploy ~/.ssh;  // 分组
    
chmod 700 ~/.ssh;               // 700访问权限

chmod 600 ~/.ssh/authorized_keys; // 600访问权限
```
工作完成了！现在，你在本地设备中应该无需输入密码就能通过 SSH 登陆远程服务器了。
> 如果所有用户共用一个账户,把所有用户的公钥复制放到共用账户下的 `.ssh/authorized_keys` 文件中。

#### 禁用密码，禁止跟用户登陆

下面我们让远程服务器再安全一些。我们要禁止所有用户通过密码认证登陆，还要禁止根用户登陆。记住，根用户能做任何事，所以我们要尽量不让根用户访问服务器。

以 `deploy` 用户的身份登陆远程服务器，然后打开 `/etc/ssh/sshd_config` 文件。这是 SSH 服务器软件的配置文件。

找到 `PasswordAuthentication` 设置，将其值修改为 `no`；如果需要，去掉这个设置的注释。

然后找到 `PermitRootLogin` 设置，将其值修改为 `no`;如果需要，去掉这个设置的注释。

保存改动，重启 SSH 服务器

##### Ubuntu
```
sudo service ssh  restart
```

##### CentOS
```
sudo systemctl restart sshd.service
```

> 如果想要让根用户登陆，在任何一个账户下把上面的那些配置改回来就行了。

### 解决ssh登录后闲置时间过长而断开连接

##### 方法一(推荐)
这个是最推荐的方法,其他的可能会出问题

找到 `/etc/ssh/sshd_config` 文件 `ClientAliveInterval 0` 和 `ClientAliveCountMax 3` 并将注释符号（"#"）去掉。

- 将 `ClientAliveInterval` 对应的 `0` 改成 `60`;
    
    ```
    ClientAliveInterval 指定了服务器端向客户端请求消息 的时间间隔, 默认是0, 不发送,ClientAliveInterval 60 表示每分钟发送一次,
    然后客户端响应, 这样就保持长连接了
    ```
- `ClientAliveCountMax` 使用默认值 `3` 即可;
    
    ```
    ClientAliveCountMax 表示服务器发出请求后客户端没有响应的次数达到一定值, 就自动断开.正常情况下, 客户端不会不响应.
    ```

重起 `sshd` 服务：
```
service sshd restart
```



##### 方法二

修改 `/etc/ssh/sshd_config` 配置文件，找到 `ClientAliveCountMax`（单位为分钟）修改你想要的值。 
修改完成执行：
```
service sshd reload 
```

##### 方法三
找到所在用户的.ssh目录，如root用户该目录在：/root/.ssh/，在该目录创建config文件

```
vi /root/.ssh/config
```
加入下面一句：
```
ServerAliveInterval 60
```
保存退出，重新开启 root 用户的 shell，则再 ssh 远程服务器的时候，不会因为长时间操作断开。应该是加入这句之后，ssh 客户端会每隔一段时间自动与 ssh 服务器通信一次，所以长时间操作不会断开。

##### 方法四
修改 `/etc/profile` 配置文件；增加：

```
TMOUT=1800
```
这样30分钟没操作就自动LOGOUT


