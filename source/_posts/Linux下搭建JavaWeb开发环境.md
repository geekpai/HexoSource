---
title: Linux下如何搭建 Java Web开发环境
date: 2016-09-2 23:46:29
tags: [aliyun,Linux]
categories: Linux
---

### Linux下如何搭建 Java Web开发环境 

​	这篇文章教大家如何在Linux环境下搭建Java Web开发环境，包括配置 JDK 、Tomcat 和 Mysql，验证版本为CentOS release 6.8，（查询版本信息：`cat /etc/issue`）仅供参考！

<!-- more -->

### 一、安装 JDK

​	JDK 是开发Java程序必须安装的软件

1. 首先查看一下 `yum` 源里面的 JDK：`yum list java*`
2. 选择适合本机的JDK，并安装：`yum install java-1.7.0-openjdk* -y`
3. 安装完成后，查看是否安装成功：`java -version`

### 二、安装 Tomcat

​	Tomcat 是一个应用服务器，是开发和调试 jsp 程序的首选，可以利用它来响应 HTML 页面的访问请求。

1. 进入本地文件夹：

   ```shell
   cd /usr/local
   ```

2. 到官网找到 Tomcat 的下载链接，并下载到服务器中, 这里提供了一个快速下载 Tomcat 的地址：

   ```shell
   wget https://mc.qcloudimg.com/static/archive/fa66329388f85c08e8d6c12ceb8b2ca3/apache-tomcat-7.0.77.tar.gz
   ```

3. 解压这个文件夹：

   ```shell
   tar -zxf apache-tomcat-7.0.77.tar.gz
   ```

4. 重命名这个文件：

   ```shell
   mv apache-tomcat-7.0.77 /usr/local/tomcat7
   ```

5. 进入 bin 文件夹：

   ```shell
   cd /usr/local/tomcat7/bin
   ```

6. 给这个文件夹下的所有 shell 脚本授予权限：

   ```shell
   chmod 777 *.sh
   ```

7. 开启tomcat服务：

   ```shell
   ./startup.sh
   ```

8. 访问Tomcat

   此时，访问对应自己ip的[http://xxx.xxx.xxx.xxx:8080]() 可访问到刚才启动的 Tomcat 的内置示例页面则表示Tomcat安装成功。


### 三、安装 MySQL

1. 使用 `yum` 安装 MySQL：`yum install -y mysql-server mysql mysql-devel`

2. 安装完成后，启动 MySQL 服务：`service mysqld restart`

   【注】：解决CentOS 7 无法启动mysql ，报如下错的解决办法：

   ```shell
   Redirecting to /bin/systemctl restart  mysqld.service
   Failed to restart mysqld.service: Unit not found.
   ```

   > 执行以下三条命令即可

   ```shell
   yum install mariadb-server -y
   安装成功后，将其加入开机启动
   systemctl enable mariadb.service
   启动mysql服务进程
   systemctl start mariadb.service
   ```

3. 设置 MySQL 账户 root 密码：`/usr/bin/mysqladmin -u root password 'xxxxx'`

   【附】: 配置mysql（设置密码等）:`mysql_secure_installation`

   ```shell
   NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
         SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

   In order to log into MySQL to secure it, we'll need the current
   password for the root user.  If you've just installed MySQL, and
   you haven't set the root password yet, the password will be blank,
   so you should just press enter here.

   Enter current password for root (enter for none): 
   OK, successfully used password, moving on...

   Setting the root password ensures that nobody can log into the MySQL
   root user without the proper authorisation.

   Set root password? [Y/n] y                             [设置root用户密码]
   New password: 
   Re-enter new password: 
   Password updated successfully!
   Reloading privilege tables..
    ... Success!

   By default, a MySQL installation has an anonymous user, allowing anyone
   to log into MySQL without having to have a user account created for
   them.  This is intended only for testing, and to make the installation
   go a bit smoother.  You should remove them before moving into a
   production environment.

   Remove anonymous users? [Y/n] y                        [删除匿名用户]
    ... Success!

   Normally, root should only be allowed to connect from 'localhost'.  This
   ensures that someone cannot guess at the root password from the network.

   Disallow root login remotely? [Y/n] y                  [禁止root远程登录]
    ... Success!

   By default, MySQL comes with a database named 'test' that anyone can
   access.  This is also intended only for testing, and should be removed
   before moving into a production environment.

   Remove test database and access to it? [Y/n] y         [删除test数据库]
    - Dropping test database...
   ERROR 1008 (HY000) at line 1: Can't drop database 'test'; database doesn't exist
    ... Failed!  Not critical, keep moving...
    - Removing privileges on test database...
    ... Success!

   Reloading the privilege tables will ensure that all changes made so far
   will take effect immediately.

   Reload privilege tables now? [Y/n] y                    [刷新权限]
    ... Success!
   ```


 
