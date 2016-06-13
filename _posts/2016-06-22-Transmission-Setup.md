---
layout: post
title: Transmission－Raspbianx系统中的配置 
categories: [blog ]
tags: [Tool, Mac,]
description: 在树莓派中配置Transmission。
---

## Raspbian系统中Transmission配置

1. 更新软件仓
    'sudo apt-get update'
 
 2. 安装Transmission
    'sudo apt-get install transmission transmission-daemon'
    
 3. 修改Transmission配置文件（修改前请备份）
    'sudo nano /etc/transmission-daemon/settings.json'
    
    一般修改以下内容即可:
    '''
    "download-dir": "下载完成的目录",
    "incomplete-dir": "下载未完成的目录",
    "rpc-enabled": true,
    "rpc-username": "username",
    "rpc-password": "password",
    "rpc-port": 9091,
    "umask": 18
    "rpc-authentication-required": true, --远程管理认证需要:是
		"rpc-enabled": true, -- 远程管理功能打开:是
		"rpc-whitelist": "*.*.*.*", --白名单IP,全部改为*（或者根据需要更改）
		"rpc-whitelist-enabled": false,--使用白名单:否（如果是，则使用白名单）
    '''
    其他部分的设置，如链接限制，下载限制等可根据情况修改。
         
 4. 给Transmission设置最高权限（防止出现下载一会就Permission Denied）
    'sudo chmod 777 -R /var/lib/transmission-daemon'
    
    可选：如果还存在权限问题，添加你所使用的常用用户username到transmission用户组
    'sudo gpasswd -a username transmission'
 
 5. 启动Transmission服务
    'sudo service transmission-daemon reload'
		'sudo service transmission-daemon restart'
 
 6. 使用Transmission
    在浏览器中访问 IP 加 9091端口：比如：'http://192.168.1.100:9091/' 。访问时输入上面配置的用户名'username'和密码'password'。
    现在，将BT站点种子文件上传到transmission网页上，就可以自动下载了。
