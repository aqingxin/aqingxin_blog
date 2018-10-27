---
layout: post
title: 'NodeJs的mysql操作'
subtitle: 'NodeJs的直连mysql操作和使用连接池'
date: 2018-10-27
categories: 技术
tags: NodeJs
---

##### 首页需要先安装mysql模块

```
cnpm install mysql --save
```

##### 然后在js文件引入该模块

```javascript
var mysql=require('mysql');
```


### 直连mysql
##### 创建一个数据库连接
```javascript
var connection=mysql.createConnection({   //创建数据库连接
	host:'localhost',    //主机地址
  user:'root',         //用户
	password:'',         //密码
  database:'website',  //数据库名
})
```

##### 然后就是执行你想要执行的sql语句
```javascript
let selectSql="SELECT * FROM users WHERE user_name=123";

connection.query(selectSql,function(err,ress){   //回调函数的第一个参数一般为err，这也称为错误优先的回调函数
    if(err){
      console.log(err)
    }else{
      console.log(ress)
    }
})
```

### 使用mysql的连接池

##### 创建一个连接池
```javascript
var pool=mysql.createPool(...)    //括号里的参数和直连mysql的参数一样
```

##### 然后是获取连接,执行sql语句
```javascript
pool.getConnection(function(err,connection){
    connection.query(selectSql,function(err,result){   //执行sql语句
        if(err) throw err   //如果报错，则输出报错
        
        console.log(result);
        res.json({msg:result});
        connection.release();   //当连接完成后，释放到池中
    })
})
```