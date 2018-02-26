---
title: aria2配置
date: 2018-02-17 23:39:07
tags: 配置
---

> aria2 is a lightweight multi-protocol & multi-source, cross platform download utility operated in command-line. It supports HTTP/HTTPS, FTP, SFTP, BitTorrent and Metalink.

<!-- more -->

### 传送门 

* [aria2](https://github.com/aria2/aria2)
* [YAAW](https://github.com/binux/yaaw)
* [BaiduExporter](https://github.com/acgotaku/BaiduExporter)
* [说明](https://blog.icehoney.me/posts/2015-01-31-Aria2-download)
* [网盘](https://pan.baidu.com/s/1mjFf8xU) 密码:7oe3


### 过程

#### 下载

```bash
brew install aria2
```

#### 启动服务

```bash
# ./aria2c.conf是配置文件相对于当前命令所在位置
# -D 让服务常驻，启动完即可关闭终端
aria2c --conf-path=./aria2c.conf -D
```

#### 用YAAW管理下载

chrome商店搜索YAAW，安装。YAAW扩展程序按钮点开，页面右上角小扳手配置aria2本地服务端口`http://localhost:6800/jsonrpc`。

#### 用BaiduExporter直接从百度网盘下载

插件暂时只能手动安装。下载`BaiduExporter.crx`，打开chrome扩展工具界面，拖进去。网页上打开百度网盘页面，工具栏多出一个*导出下载*按钮可供操作。
