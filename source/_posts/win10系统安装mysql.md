title: win10系统安装mysql
author: weiyonghua
tags:
  - mysql
categories:
  - 环境部署
date: 2019-12-31 15:52:00
---
## Step1

官方下载地址 [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

选择手动下载版本 mysql-5.7.25-winx64.zip

 

![](https://img2018.cnblogs.com/blog/1173783/201902/1173783-20190201131538870-392579130.png)

 

 

 解压到自己指定的路径

 

![](https://img2018.cnblogs.com/blog/1173783/201902/1173783-20190201131744298-516498007.png)

 

 

上图中的my.ini及data文件夹在压缩包里是没有的，后面需要自己添加

 

my.ini如下(注意目录路径必须用\\不能用\不然报错)，直接copy~

 

[client]
 port=3306
 default-character-set=utf8

[mysqld]
 # 设置为自己MYSQL的安装目录
 basedir=C:\\nova_work_software\\mysql-5.7.25-winx64
 # 设置为MYSQL的数据目录
 datadir=C:\\nova_work_software\\mysql-5.7.25-winx64\\data
 port=3306
 character_set_server=utf8
 sql_mode=NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER
 #开启查询缓存
 explicit_defaults_for_timestamp=true
 skip-grant-tables

 

然后在目录下创建一个data文件夹

 

### Step2

**设置环境变量**

电脑->属性->高级系统属性->环境变量 
 在系统变量里的Path中新建(%MYSQL安装目录%\bin)

![](https://images2018.cnblogs.com/blog/1421883/201808/1421883-20180803090055794-1170805393.png)

 

### Step3

进入Mysql安装目录下的bin文件夹，在此处**以管理员身份打开cmd**

执行 mysqld --initialize

 ![](https://img2018.cnblogs.com/blog/1173783/201902/1173783-20190212013518801-914896084.png)

 

这句命令是为了使data目录下有正常的mysql文件夹和相关文件。

若出现error:Found option without preceding group in config file: D:\Mysql\mysql-5.7.19-winx64\my.ini at line: 1

解决方法是，把my.ini保存为ANSI格式

![](https://images2018.cnblogs.com/blog/1421883/201808/1421883-20180803090357703-933868986.png)

 

接着依次执行下面命令（管理员模式）：

mysqld install 
 net start mysql

 ![](https://images2018.cnblogs.com/blog/1421883/201808/1421883-20180803090430112-1468463322.png)

 

 

因为my.ini中加入了skip-grant-tables配置，所以可以直接使用  mysql -u root -p     输入任意密码登录

 ![](https://img2018.cnblogs.com/blog/1173783/201902/1173783-20190212023213515-1109275908.png)

 

然后通过SQL语句修改root用户的密码；

    #将数据库切换至mysql库mysql> USE mysql;#修改密码mysql> update user set authentication_string=PASSWORD('123456') where user='root';#刷新MySQL权限相关的表mysql> flush privileges;mysql>exit;

 

修改完密码后，把my.ini中的#skip-grant-tables 注释掉然后net stop mysql和net start mysql重启mysql服务

然后就可以mysql -uroot -p123456登录了

 

登录后执行show databases;

可能会报错  ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

 

需要用  alter user user() identified by "123456";  再改一次密码