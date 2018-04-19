---
title: hexo tips
date: 2017-03-12 15:05:19
tags: #配置
---

> 记录用hexo搭建blog过程中的配置技巧

<!-- more -->

# 发布配置

#### 1.常用命令

* 启动服务 `hexo server`
* 发布部署 `hexo g -d`. 需要配置`deploy`
```yml
# 开发时将分支切换到dev，部署在master
# 同时gitignore中配置public/  themes/ 目录忽略
deploy:
  type: git
  repo: https://github.com/lefreet/lefreet.github.io.git
  branch: master
```

#### 2. md中的图片资源管理

配置中启用`post_asset_folder: true`. 这样，当通过`hexo new`命令创建一篇文章时，会在`_post`目录下创建一个同名文件夹。文章内的图片通过md原生的语法`![assets_name](assets_name.xxx) `到对应图片，并且发布后路径会自动同步变更。
