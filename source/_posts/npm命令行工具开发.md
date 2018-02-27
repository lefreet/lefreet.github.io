---
title: npm命令行工具开发
date: 2017-09-11 10:21:11
tags: 指南
---

> 最近琢磨开发[geojson-help](https://github.com/lefreet/geojson-help)命令行工具，把开发工具的过程整理了下

<!-- more -->

* 初始化
```bash
mkdir project
npm init
```

* 新建`bin/run`命令文件，内容如下，代表在node环境执行
```bash
#!/usr/bin/env node
```

* 修改下可执行权限
```bash
$ sudo chmod u+x run
```

* 修改package.json文件，注册npm命令
```json
"bin": {
  "run": "./bin/run"
}
```

* 软链接命令到全局环境，方便调试
```bash
npm link
```

* 然后在run文件中写命令行代码，可以用[commander](https://github.com/tj/commander.js)来执行git风格的命令
```js
var run = require('../package.json');
var commander = require('commander');

commander
  .version(run.version)
  .usage('<command> [options]')
  .command('init', 'initialize')
  .parse(process.argv)
```

* 同样新建`bin/run-init`, 执行`run init` 时即可将命令分发到`run-init`文件内

* 在`run-init`中写一些要在node环境中执行的代码，可以通过`commander`来抓取参数
```js
var commander = require('commander');

commander
  .option('-p, --params <params>', 'some params')
  .parse(process.argv);
  
console.log(params.params);
```

* 执行`run init -p test`， 控制台应该会输出`test`
* 将npm包发布后， `npm i your-package -g`安装到全局，即可直接调用命令

