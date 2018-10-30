---
layout: post
title: 'NodeJs中的流(Stream)'
subtitle: 'NodeJs中的流(Stream)'
date: 2018-10-30
categories: 技术
tags: NodeJs
---

- 从流中读取数据
- 写入流
- 管道流
- 链式流


## 从流中读取数据
创建input.txt，内容如下
```javascript
菜鸟教程官网地址：www.runoob.com
```

创建main.js文件，代码如下
```javascript
var fs = require('fs')   //引入模块
var data='';

//创建可读流
var readerStream=fs.createReadStream('input.txt');

//设置编码为utf8
readerStream.setEncoding('UTF8');

//处理事件 -->data,end, and error

readerStream.on('data',function(chunk){
    data+=chunk;
})

readerStream.on('end',function(chunk){
    console.log(data);
})
readerStream.on('error',function(err){
    console.log(err.stack);
})

console.log('程序执行完毕');
```
以上代码执行结果如下

```javascript
程序执行完毕
菜鸟教程官网地址：www.runoob.com
```


## 写入流
创建main.js文件，代码如下
```javascript
var fs=require('fs')
var data='菜鸟教程官网地址：www.runoob.com';

//创建一个可以写入的流，写入到文件output.txt中
var writerStream=fs.createWriteStream('output.txt')

//使用utf8编码写入数据
writeStream.write(data,'UTF8');

//标记文件末尾
writerStream.end();

//处理事件 --->data,end,error
writerStream.on('finish',function(){
    console.log('写入完成')
})
writerStream.on('error',function(err){
    console.log(err.stack)
})

console.log('程序执行完毕')
```

以上代码会见data变量的数据写入到output.txt文件中，代码执行结果如下：
```javascript
程序执行完毕
写入完成
```
查看outpt.txt文件的内容
```javascript
菜鸟教程官网地址：www.runoob.com
```


## 管道流
管道流提供了一个输出流到输入流的机制。通常我们用于重一个流中获取数据并将数据传递到另一个流中。

设置input.txt文件内容如下：
```javascript
菜鸟教程官网地址：www.runoob.com
管道流操作实例
```
创建main.js文件，代码如下：
```javascript
var fs=require('fs');

//创建一个可读流
var readerStream-fs.createReadStream('input.txt');

//创建一个可写流
var writerSteam=fs.createWriteStream('output.txt');

//管道读写操作
//读取input.txt文件内容，并将内容写入到output.txt文件中
readerStream.pipe(writerStream);

console.log('程序执行完毕')

```

代码执行结果如下：
```javascript
程序执行结果如下
```

查看output.txt文件的内容：
```javascript
菜鸟教程官网地址：www.runoob.com
管道流操实例
```


## 链式流
链式流是通过连接输出流到另一个流并创建多个对个流操作流的机制。链式流一般用于管道操作。

接下我们就是用管道和链式来压缩和解压文件。

创建compress.js文件，代码如下：

```javascript
var fs=require('fs');
var zlib=require('zlib');

//压缩input.txt文件为input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));

console.log('文件压缩完成');
```
代码执行结果如下：
```javascript
文件压缩完成
```

执行完以上操作后，我们可以看到当前目录下生成了 input.txt 的压缩文件 input.txt.gz。

接下来，让我们来解压该文件，创建 decompress.js 文件，代码如下：

```javascript
var fs = require("fs");
var zlib = require('zlib');

// 解压 input.txt.gz 文件为 input.txt
fs.createReadStream('input.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('input.txt'));

console.log("文件解压完成。");
```
代码执行结果如下：
```javascript
文件解压完成。
```

