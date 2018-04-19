---
title: iis动态压缩设置
date: 2017-04-19 14:07:12
tags: #配置
---

> 增加服务器压力，减少带宽，极大压缩资源请求时间

<!-- more -->

#### 1.安装动态压缩服务
打开`服务器管理器`->`角色`->`Web服务器(IIS)`，右侧找到`添加角色服务`，在打开的窗口中找到`性能`，勾选`动态压缩`，然后安装。

![安装服务](install.png)

#### 2.配置压缩目标
打开`C:\Windows\System32\inetsrv\config\applicationHost.config`，找到`dynamicTypes`节点，添加需要的`mime`类型，常见的`json`和`javascript`如下：

```xml
  <add mimeType="application/json" enabled="true" />
  <add mimeType="application/javascript" enabled="true" />
```

#### 3.添加动态压缩对文件的操作权限
打开iis，找到对应站点，右键`编辑权限`->`添加`->`高级`，找到`IUSER`、`IIS_IUSERS`、`NETWORK_SERVICE`权限添加。然后打开站点的`功能视图`找到`压缩`，将`启动动态内容压缩`勾选，然后保存。

#### 4.重启检查
点击左侧树根节点，再右侧点击重启（注意不是重启站点），目的是使`applicationHost.config`刷新。 检查接口的`Response Headers`，看是非有`Content-Encoding:gzip`，有的话表示成功。

![重启](restart.png)

![结果](result.png)

#### 5.注意事项
* png等图片资源本身已经是无损压缩的，无需配置