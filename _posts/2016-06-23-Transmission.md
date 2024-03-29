---
layout: post
title: Transmission－Raspbianx系统中的配置 
categories: [blog ]
tags: [Tool, Mac,]
description: 在树莓派中配置Transmission。
---

拥有了 Aria2 的默默扶持，再也不用待见不用忍受 OSX 下国内那些半死不活的下载工具了。

## 关于 Aria2 

[Aria2](https://aria2.github.io/) 是一个基于命令行的开源下载工具，支持多协议、多来源（Http/https、FTP、Magnet、BitTorrent 等）、多线程的下载。

主要优势如下：

* 高速，自动多线程下载；
* 断点续传；
* 轻量。占用内存非常少，通常情况平均 4~9MB 内存占用（官方介绍）；
* 多平台。支援 Win/Linux/OSX/Android 等操作系统下的部署；
* 模块化。分段下载引擎，文件整合速度快；
* 支持 RPC 界面远程；
* 全面支持 BitTorrent 协议；


## Install Aria2

安装 Aria2 依赖于 Mac 下的包管理工具 Homebrew（Linux 用户可使用 apt-get 命令直接安装）。而 Homebrew 的使用依赖于 OSX 的 XCode 命令行工具部分。所以，这些都得有...（Linux 用户直接看第 2 小节）。

1. 先安装 Homebrew:
  * 打开终端（Terminal or iTerm）
  * 安装 XCode(已安装可跳过，选装 XCode 的 CLI 工具)：`$ xcode-select --install`
  * 复制并运行 Homebrew 安装命令：```$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```
  * 查看版本（确认安装是否完成）:`$ brew --version` 
2. Homebrew 管理工具加入系统后，安装 Aria2：
  * OSX 运行：`$ brew install aria2`
  * Linux 运行: `$ sudo apt-get install aria2` 
3. 查看命令安装情况：
  * `$ aria2c --version` 提示版本及说明则表示已安装完成。

## 配置 Aria2

命令安装完，进入 Aria2 的配置部分。

1. 在系统主目录下新建 `.aria2` 文件夹。
  * `$ mkdir .aria2`

2. 在 .aria2 文件夹下新建 `aria2.conf` 配置文件。
  * 进入文件夹：`$ cd .aria2`
  * 创建配置文件：`$ touch aria2.conf`

3. 在 Finder 里查看并对文件进行编辑。
  * a. iTerm/Terminal 当前路径下(`~/.aria2/`)输入命令 `$ open .` 进入 Finder 界面下 .aria2 文件夹；
  * b. Finder 界面，快捷键 Command + Shift + G 在弹出的路径跳转小窗口，填入 `~/.aria2/aria2.conf` 后 Return。

4. 使用 TextMate/Sublime Text/TextEdit 等编辑器打开 aria2.conf 文档，填入内容。

```
#用户名
#rpc-user=user
#密码
#rpc-passwd=passwd
#上面的认证方式不建议使用,建议使用下面的token方式
#设置加密的密钥
#rpc-secret=token
#允许rpc
enable-rpc=true
#允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true
#允许外部访问，false的话只监听本地端口
rpc-listen-all=true
#RPC端口, 仅当默认端口被占用时修改
rpc-listen-port=6800
#最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=5
#断点续传
continue=true
#同服务器连接数
max-connection-per-server=5
#最小文件分片大小, 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
#单文件最大线程数, 路由建议值: 5
split=10
#下载速度限制
max-overall-download-limit=0
#单文件速度限制
max-download-limit=0
#上传速度限制
max-overall-upload-limit=0
#单文件速度限制
max-upload-limit=0
#断开速度过慢的连接
#lowest-speed-limit=0
#验证用，需要1.16.1之后的release版本
#referer=*
#文件保存路径, 默认为当前启动位置
dir=/Users/YourID/Downloads
#文件缓存, 使用内置的文件缓存, 如果你不相信Linux内核文件缓存和磁盘内置缓存时使用, 需要1.16及以上版本
#disk-cache=0
#另一种Linux文件缓存方式, 使用前确保您使用的内核支持此选项, 需要1.15及以上版本(?)
#enable-mmap=true
#文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
#所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支持
file-allocation=prealloc
```

注意将配置表中保存路径一项「YourMacID」替换为个人的 Mac OSX 用户名（不知道可以用 `$ pwd` 命令查看）。

保存并退出。

## Aria2 的使用

配置完成后，就可以开始使用了。

Aria2 有两种模式：

* 命令直接调用。调用命令 `$ aria2c "download.url"` 下载完成后退出
* 后台常驻触发，也就是 rpc server 模式。Aria2 作为后台常驻程序，监测 rpc 端口的活动情况，添加并下载文件。完成后继续在后台运行。

涉及到命令输入，力求简化，第二种模式明显更省事。

启动 Aria2 rpc 模式的命令：

`$ aria2c --conf-path=<Path>`

`<Path>` 是指配置文件所在的绝对路径。依照上述配置一路下来，具体是：

`$ aria2c --conf-path="/Users/YourMacID/.aria2/aria2.conf" -D`

使用时注意将其中的「YourMacID」替换为个人的 Mac OSX 用户名。

这时，正确无误的话，Aria2 就启动了。

安装扩展程序，方便浏览器中查看和下载资源时的 rpc 端口调用。 

[BaiduExporter - Chrome 网上应用店](https://chrome.google.com/webstore/detail/baiduexporter/mjaenbjdjmgolhoafkohbhhbaiedbkno/related) 主要用于 Mac 下下载百度云的资源。

在网站内可见一个「导出下载」的按钮。点击可见 RPC 导出下载和设置三个选项。下载点击 RPC 选项即可。

界面：

![](http://dreamofbook.qiniudn.com/Aria2DownloadClick.png)

如果点击后发现下载没执行或报错，注意检查：

1. aria2.conf 文件配置是否出错；
2. 代理工具是否切换到全局穿越模式，如有，则退出全局模式再重试；

从后台关闭 Aria2。在终端执行 `kill` 命令来关闭 Aria2。

  * 输入 `$ kill aria2c` 点 Tab 键，显示 aria2 的进程号，然后 Return。Aria2 的进程将被关闭。
  * 直接 `$ killalll aria2c` 关闭进程。

再次使用时需重新运行。

## 搭配 Aria2 Web UI

Aria2 不带 GUI 界面。了解下载进度会有不便，日常使用需搭配 Web UI 工具方便查看。

进入网址查看：[Aria2 Web 控制台](http://aria2c.com/)（可保存为书签使用。）

界面效果：

![](http://dreamofbook.qiniudn.com/Aria2ControlUI.png)


***

## 优化启动

利用命令行工具的 alias 进行命令优化，简化 Aria2 命令的输入。

1. VIM 编辑全局配置文件：
  * 命令行工具默认：`$ vi ~/.bashrc`
  * 配置 zsh 的命令行工具：`$ vi ~/.zshrc` 
2. 在末尾添加一条新的 alias：
  * `alias aria="aria2c --conf-path="/Users/YourID/.aria2/aria2.conf" -D"` 
3. 使之全局生效：
  * 命令行工具默认：`$ source ~/.bashrc`
  * 配置 zsh 的命令行工具：`$ source ~/.zshrc` 
4. 以后每次开机或重新启用终端后，键入以下命令直接启用 Aria2，使它常驻后台：
  * `$ aria`
 

## 感谢

* [Aria2](https://aria2.github.io/)
* [雪月秋水](https://blog.icehoney.me/about) 开发的 BaiduExporter
* [Homebrew](http://brew.sh/)
 
## 参考资料

* [Homebrew 使用入门 - Microdust](http://azeril.me/blog/Homebrew.html)
* [使用 Aria2 下载百度网盘和 115 的资源](https://blog.icehoney.me/posts/2015-01-31-Aria2-download)
* [aria2配置示例 - Binuxの杂货铺](http://blog.binux.me/2012/12/aria2-examples/) * [linux 高速下载工具 aria2 的用法 - myweishanli的专栏 - 博客频道 - CSDN.NET](http://blog.csdn.net/myweishanli/article/details/42221481)