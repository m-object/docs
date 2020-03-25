# CentOS 7.5 使用 yum 方式安装 MySQL 5.7

> 环境：CentOS 7.5 MySQL 5.7

#### 1. 准备工作

CentOS 7 的 yum 源中默认是没有 MySQL 的，因为现在的 CentOS 7 中默认是安装 mariaDB 的。

首先应该将 MySQL 加入我们的 CentOS yum 源中。

首先下载 MySQL 的 repo 源：

```bash
[root@master /]# wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm		#这个网址可以直接复制使用也可以去 mysql 官网找。
[root@master /]# yum localinstall mysql57-community-release-el7-8.noarch.rpm 	#将mysql 加入 yum 源
[root@master /]# yum repolist enabled | grep "mysql.*-community.*"　　　　　＃查看 mysql 源
```

#### 2. 安装

```bash
[root@master /]# yum install mysql-community-server					#安装、等待
[root@master /]# systemctl start mysqld    							#启动服务
[root@master /]# systemctl status mysqld							#查看状态

[root@master /]# systemctl enable mysqld			
[root@master /]# systemctl daemon-reload							#开机启动
[root@master /]# grep 'temporary password' /var/log/mysqld.log		#查看初始密码
2019-07-08T09:38:17.049056Z 1 [Note] A temporary password is generated for root@localhost: b,E+i>Sd+2*,
```

#### 3. 登入、操作

```
[root@master /]# mysql -uroot -p						#登录 mysql
Enter password: 										#这里输入上面查询到的初始密码就行
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '你要改的密码';	#修改密码
```

修改密码这里可能会遇到的问题就是你设置的密码太过简单，会提示你当前密码不符合密码策略。设置的复杂一点就可以了

打开 MySQL 远程连接功能：

登录到 MySQL 中，更改操作的数据库到 mysql ；

```bash
mysql> use mysql 
mysql> GRANT ALL PRIVILEGES ON *.* TO '账号'@'%' IDENTIFIED BY '密码';        执行此命令即可，账号密码用自己的 mysql 账号密码
mysql> flush privileges; 										刷新权限
mysql> select  User,authentication_string,Host from user ;			查询用户表
+---------------+-------------------------------------------+-----------+
| User          | authentication_string                     | Host      |
+---------------+-------------------------------------------+-----------+
| root          | *EEE371AF14C5B26223BF194DF260E7E5460981DD | localhost |
| mysql.session | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | localhost |
| mysql.sys     | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | localhost |
| root          | *EEE371AF14C5B26223BF194DF260E7E5460981DD | %         |
+---------------+-------------------------------------------+-----------+
```

> 最后一行就是我设置的，%表示任何机器都能访问我的 MySQL ，前提是通过 root 用户。





**尊重他人**

转载地址：<https://blog.csdn.net/wchenjt/article/details/95218771>