---
layout: post
title: '小程序语音识别demo'
subtitle: '小程序结合百度SDK、Node做一个语音识别的deom'
date: 2018-12-10
categories: 技术
tags: webpack
---

webpack在启动后会从配置的入口模块触发找出所有依赖的模块，Resolve配置webpack如何寻找模块对应的文件。webpack内置JavaScript模块化语法解析功能，默认会采用模块化标准里约定好的规则去寻找，但你可以根据自己的需要修改默认的规则。

### 1.module
commonjs中，找包的顺序是从当前的node_modules下开始寻找，找不到会从外层再去寻找，我们可以使用module字段来指定找包的范围。

```javascript
resolve: {
  module: [path.resolve('node_module')] 
},

```

### 2.alias
引包时，一般找包的package.json中的“main”字段，但有的情况下，你可能并不想使用main引用的文件，比如我们只要使用bootstrap的样式，import “bootstrap”；

```javascript
// bootstrap 的package.json
"style": "dist/css/bootstrap.css",
"sass": "scss/bootstrap.scss",
"main": "dist/js/bootstrap",
"repository": {
  "type": "git",
  "url": "git+https://github.com/twbs/bootstrap.git"
},
```
可以看到，默认却引的是js，那么我们只能手动的改写引用方式
```javascript
import 'bootstrap\dist\css\bootstrap.min.css'；  //能达到效果，但不优雅，而且在每个需要的地方都得这样写 
```
resolve中提供了alias字段，方便我们为一个路径起“外号”

```javascript
import 'bootstrap\dist\css\bootstrap.min.css'；  //能达到效果，但不优雅，而且在每个需要的地方都得这样
```
### 3. mainFields
这个字段是指定找到包后，引用package.json的那个字段，它是一个数组，如果没有这个字段，那么依次查找，找到后引用，因此上例可以这样：

```javascript
resolve: {
    // modules: ["node_module"], //当前目录的node_module
    mainFields: ["style", "main"],    // 因为bootstrap中style指向的才是css文件
    // alias: {
    //   bootstrap: "bootstrap/dist/css/bootstrap.css"
    // }
  },
```
### 4. mainFiles
也是一个数组，指的是入口文件的名字，默认为index.js

### 5. extensions

现在，引入文件的时候，必须带后缀，但是我们可以配置这个字段来指定默认后缀查找顺序

```javascript
extensions: [.js, css, .json]  //如果这样  import 'index',那么寿县找‘index.js’,找不到后会找‘index.css’,以此类推
```
