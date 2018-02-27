---
title: 用verdaccio搭建私有npm包管理平台
date: 2018-02-25 18:32:53
tags: 指南
---

> 之前给公司内部搭建了个私有包管理平台，记录下过程

<!-- more -->

### 介绍

[verdaccio](https://github.com/verdaccio/verdaccio)是一个私有的[npm](https://www.npmjs.com/)包管理平台，它的前身是[sinopia](https://github.com/rlidwka/sinopia)，目前sinopia已经停止维护，见[#376](https://github.com/rlidwka/sinopia/issues/376)。

verdaccio在npm官方包管理功能的基础上，提供了私有包管理、包源镜像、重写公共包等功能扩展，支持多种平台和环境进行部署。

> npm本身是提供[作用域包](https://docs.npmjs.com/getting-started/scoped-packages)的特性，但是私有包的功能收费的，对于公司而言，自己搭建平台比较实惠。

### 平台搭建(windows server 2012)

* [参考文档](https://github.com/verdaccio/verdaccio/blob/master/docs/windows.md)

1. 在部署路径创建个目录，如`d:\demo`，进入该目录，执行`npm install verdaccio`安装。
2. 同目录下创建配置文件`config.yaml`，默认配置从[这里](https://github.com/verdaccio/verdaccio/blob/master/conf/default.yaml)拷贝一份。主要修改几个地方：
```yaml
# 设置上游源
uplinks: 
  npmjs:
    url: https://registry.npmjs.org
  taobao:
    url: https://registry.npm.taobao.org
# 设置上游源顺序以及匹配条件
packages:
  ...:
    proxy:
      - taobao
      - npm
# 设置外网监听端口，否则无法外网访问
listen: http://0.0.0.0:10001
```
3. 下载[NSSM](https://www.nssm.cc/download/)到该目录，作用是让verdaccio作为常驻后台服务。命令行窗口打开转到该目录，执行`nssm install`回车，弹出对话框对应输入：
  * Path: `node`
  * Startup directory: `d:\demo`
  * Arguments: `d:\demo\node_modules\verdaccio\src\lib\cli.js -c d:\demo\config.yaml`
  * Server name: `my-verdaccio`

4. 接上，执行`nssm start my-verdaccio`，服务就跑起来了
5. 试着外网访问`ip+host`，注意防火墙问题。出别的问题自己看[官方文档](https://github.com/verdaccio/verdaccio/tree/master/docs)，我也是乱写的。
6. 官方默认用户密码加密方式[参考](https://github.com/verdaccio/verdaccio/blob/master/src/plugins/htpasswd/crypt3.js)
```js
crypt3('123456') // -->$65I6KH/.1F1w
```

### 包源管理

推荐用[nrm](https://github.com/Pana/nrm)管理npm包源，当然，手动操作也是可以的，[参考](https://docs.npmjs.com/misc/config#registry)。

```bash
# 安装nrm
npm i nrm -g

# 查看可用包源
nrm ls

# 添加verdaccio包源，别名strong(按个人习惯定义)
nrm add strong http://yourweb:yourhost/

# 切换到verdaccio包源，切换一次，终身有效
nrm use strong

# 注册一个用户，然后愉快的开始npm操作
npm adduser

```
