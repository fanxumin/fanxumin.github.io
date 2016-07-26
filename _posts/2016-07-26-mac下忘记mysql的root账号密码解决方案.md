---
layout: post
title:  "mac下忘记mysql root账号的密码怎么办"
date:   2016-07-26 20:12:39
tags:
- Database
categories: database
---


 mac下忘记mysql root账号的密码怎么办
step1:
苹果->系统偏好设置->最下边点mysql 在弹出页面中 关闭mysql服务（点击stop mysql server）

step2：
进入终端输入：cd /usr/local/mysql/bin/
回车后 登录管理员权限 sudo su
回车后输入以下命令来禁止mysql验证功能 ./mysqld_safe --skip-grant-tables &
回车后mysql会自动重启（偏好设置中mysql的状态会变成running）

step3. 
输入命令 ./mysql
回车后，输入命令 FLUSH PRIVILEGES; 
回车后，输入命令 SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码');

结束！屡试不爽！


