---
title: nodejs 学习记录
tags: vscode markdown
notebook: nodejs
---

# nodejs 学习记录
## 1.process.cwd()、path、__dirname的区别
- process.cwd()指的是运行该nodejs 进程时的路径，与path.resolve()方法的返回值一致，即执行 **node xx.js** 时所在的路径，及时在子目录里面执行 **path.resolve()/process.cwd()**，获取的值仍然是 **node xxx.js**时的目录；
- dirname指调用模块文件所在的路径。
```javascript
const fs = require('fs');
const path = require('path');
console.log(fs.realpathSync(process.cwd()))
//d:\Users\git\react\react-admin
require('./config/mypath.js')
...
//config/mypath.js
console.log(__dirname)
//d:\Users\git\react\react-admin\config
```

## 2.await async
- async 方法执行会立即返回一个promise对象，然后继续执行下面的代码。async方法内部的代码是同步执行的，后面的await需要等待前面的await方法执行完后才能执行。
- await 后面可以是表达式，也可以是返回promise的执行函数；
- 重要说明：<br>
async 方法中，如果方法体内没有await，方法体内是变量或者函数声明，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。
```
async function a(){
    console.log('async inner')
    function g(){};
}

var c = a();
c.then((result)=>{
    //result 为undefined
    console.log('c then',result)
})
```
如果在async 方法中，如果方法体内没有await，**return**方法体内是变量或者函数声明，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。
```
async function a(){
    console.log('async inner')
    return function g(){};
}

var c = a();
c.then((result)=>{
    //result function g(){}
    console.log('c then',result)
})
```
如果在async 方法中，如果方法体内没有await，是一个promise对象，则执行async 方法会立即返回一个promise，且PromiseStatus为resolved。
```
async function a(){

   new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('finished')
        },2000)
     }).then((result)=>{
         console.log()
     })
}

var c = a();
c.then((result)=>{
    console.log(result)
})
```
如果在async 方法中，如果方法体内没有await，**return**是一个promise对象，则执行async 方法会立即返回一个promise，且PromiseStatus为**pending**。
```
async function a(){

   new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('finished')
        },2000)
     }).then((result)=>{
         console.log()
     })
}

var c = a();
c.then((result)=>{
    console.log(result)
})
```
如果async方法体中最后是await，则async方法立即执行完始终返回promise，且status为pending,如果在async的前面有return，则在async返回的promise的then方法中，可以获取到async内部promise的返回值。
总结，async的正确用法为，有await，且前面有return，不管需不需要，最好要这么写`return await xxxx`。
```
async function a(){
    return await new Promise((resolve,reject)=>{
        setTimeout(()=>{
//             resolve('finished')
            reject('finished')
        },2000)
     }).then((result)=>{
         console.log()
         return result;
     }).catch(err =>{
         console.log()
         return result;
     })
}

var c = a();
c.then((result)=>{
  //result 为async 内部promise then里面return 的result值。
    console.log(result)
})

```



## 3.node http模块中 end,finish 的区别
end and finish are the same event BUT on different types of Streams.
- stream.Readable fires ONLY end and NEVER finish
- stream.Writable fires ONLY finish and NEVER end
```
Why the different naming of the same event?

The only reason I could think of is because of duplex streams (stream.Duplex), which implement both stream.Readable and stream.Writable interfaces (https://nodejs.org/dist/latest-v5.x/docs/api/stream.html#stream_class_stream_duplex) are readable and writable stream at the same time. To differentiate between end of reading and end of writing on the stream you must have a different event fired. SO, for Duplex streams end is end of reading and finish is end of writing.
```

reference:[stackoverflow](https://stackoverflow.com/questions/28334610/whats-the-difference-between-end-and-finish-events-in-node-streams)

## 4.sequelize库的使用方法
- 1.创建连接
```
const Sequelize = require("sequelize");
//创建连接
const sequelize = new Sequelize("game", "root", "root", {
    host: "localhost",
    dialect: "mysql",

    pool: {
      max: 5,
      min: 0,
      idle: 10000
    }
  });
//测试连接
  sequelize
    .authenticate()
    .then(() => {
      console.log("Connection has been established successfully.");
    })
    .catch(err => {
      console.error("Unable to connect to the database:", err);
    });
```

- 2.定义model

```
//定义model
var SocialUrl = db.define(
    "SocialUrl",
    {
    videoURL: Sequelize.STRING,
    },
    {
    tableName: "social_urls",
    timestamps: false
    }
);
//同步映射
var SocialUrlPromise = SocialUrl.sync()
```

- 3.在request中处理查询
```
app.get("/insert", (req, res) => {
    SocialUrlPromise.then(()=>{
        SocialUrl.findAll().then(users => {
            res.send(users)
        });
    })
})
```

## 5.fs.read,fs.readFile,fs.createReadStream的区别

首先看一下各自需要的参数：<br>
- fs.readFile(path[, options], callback)<br>
由于readFile的参数里面没有buffer，所以，readFile是一次性把文件的所有内容读取到内存中，对于操作非常大（> 1G）的文件慎用，性能差，而且会造成内存爆棚。
```
const fs = require("fs");
const path = require("path");
let file2 = path.resolve("D", "/test/a2.log");

fs.readFile(file2, (err, data) => {
  console.log(data.length);
});
```

- fs.read(fd, buffer, offset, length, position, callback) <br>

fs.read的参数中包含buffer，因此，该方式一次只读取buffer长度的数据，读取长度，读取位置可控,多用于从文件的某个位置开始读取数据。<br>
fs.read使用需要fd，就需要先调用fs.open方法，在回调里面获取fd。
```
const fs = require("fs");
const path = require("path");
let file = path.resolve("D", "/test/a.log");
let file2 = path.resolve("D", "/test/a2.log");

fs.open(file, "r", (err, fd) => {
  let buf = new Buffer(1024);
  fs.read(fd, buf, 0, 1024, 0, (err, bytesRead, buf2) => {
    console.log(buf2.slice(0, bytesRead).toString());
  });
});
```
- fs.createReadStream(path[, options]) <br>
以流的形式读取数据，先创建流，返回ReadStream对象。ReadStream是基于事件，调用data等事件，依次读取数据。该方法主要用于读取非常大的文件，通过设置start，end也可以从某个位置开始读取，而不是直接读取整个文件。
```
const fs = require("fs");
const path = require("path");
let file = path.resolve("D", "/test/a2.log");

let readStream = fs.createReadStream(file, {
  flags: "r",
  defaultEncoding: "utf8",
  autoClose: true,
  start: 0
});

readStream.on("data", chunk => {
  writeStream.write(chunk,function(){
      console.log('ok')
  })
});
```

## 6.express session 与 redis
- express<br>
express4.X 默认情况下，没有session中间件，需要自己指定,`npm install express-session`。<br>
```
var app = express()
app.set('trust proxy', 1) // trust first proxy
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true }
}))
```
- redis<br>
将session保存在redis中，一方面可以提高服务器内存使用效率，另一方便也可以提高session性能，并且数据可以持久化在redis数据库中，`npm install connect-redis express-session`。
```
var session = require('express-session');
var RedisStore = require('connect-redis')(session);

app.use(session({
    store: new RedisStore(options),
    secret: 'keyboard cat',
    //ttl: 30
}));
```
### 6.1 redis 命令
- window下启动redis,在安装目录下执行 `redis-server.exe redis.windows.conf`
- 连接redis，在安装目录下执行 `redis-cli`
- 查看所有的keys `keys *`
- 删除当前数据库中的所有Key `flushdb`
- 删除所有数据库中的key `flushall`


redis默认持久化24小时，可以通过**ttl**设置缓存时间，单位是秒。

## 7.webpack devServer中contentBase与publicPath的区别
```
devServer: {
    contentBase: "./app",
    publicPath:"/hehe/",
    port:'3000',
    ……
  },
```
- publicPath 是为目标网站添加url前缀，如上设置，访问地址应该为：`http://localhost:3000/hehe`
- contentBase 是被webpack server服务的文件服务器，也就是说，工程下的/app/下面的文件夹内的文件都可以通过浏览器访问到。
`http://localhost:3000/hehe/assets/img/blue-bg.jpg`,`http://localhost:3000/assets/img/blue-bg.jpg` 均可以访问到。

## 8.match,exec的区别
javascript中与正则表达式有关的匹配字符串的函数主要有RegExp类的方法exec(string)以及String类的方法match(regex)。
### 8.1 exec是正则表达式的方法，而不是字符串的方法，它的参数才是字符串
```
var re=new RegExp(/\d/);
re.exec("abc4def");

或者使用perl风格：

/\d/.exec( "abc4def" );
match是字符串类提供的方法，它的参数是正则表达式对象，如下用法是正确的：

"abc4def".match(\d);
```
### 8.2 exec和match返回的都是数组
如果执行exec方法的正则表达式没有分组（没有括号括起来的内容），那么如果有匹配，他将返回一个只有一个元素的数组，这个数组唯一的元素就是该正则表达式匹配的第一个串;如果没有匹配则返回null。
```
下面两个alert函数弹出的信息是一样的：

var str= "cat,hat" ;
var p=/at/; //没有g属性
alert(p.exec(str))
alert(str.match(p))

都是"at"。在这种场合下exec等价于match。
```
但是如果正则表达式是全局匹配（g属性）的，那么以上代码结果不一样了:
```
var str= "cat,hat" ;
var p=/at/g; //注意g属性
alert(p.exec(str))
alert(str.match(p))

分别是
"at"
"at,at"。

因为exec永远只返回第一个匹配，而match在正则指定了g属性的时候，会返回所有匹配。
```
### 8.3 exec 正则带分组
如果 exec() 找到了匹配的文本，则返回一个结果数组。否则，返回 null。<br>
此数组的第 0 个元素是与正则表达式相匹配的文本;<br>
第 1 个元素是与 RegExpObject 的第 1 个子表达式相匹配的文本（如果有的话），子表达式指的是在正则表达式中，用括号包含的内容;<br>
第 2 个元素是与 RegExpObject 的第 2 个子表达式相匹配的文本（如果有的话），以此类推。<br>
除了数组元素和 length 属性之外，exec() 方法还返回两个属性。index 属性声明的是匹配文本的第一个字符的位置。input 属性则存放的是被检索的字符串 string。
### 8.4 总结
在调用非全局的 RegExp 对象的 exec() 方法时，返回的数组与调用方法 String.match() 返回的数组是相同的。
```
var str= "cat,hat" ;
var p=/at/; //没有g属性
alert(p.exec(str))
alert(str.match(p))

都弹出“at”
/***************************************/
var str= "cat2,hat8" ;
var p=/c(at)\d/;
alert(p.exec(str))
alert(str.match(p))

都弹出“cat2,at”
```
在调用全局的RegExp对象的时候，exec()的第一个元素始终返回第一个满足条件的配置值，如果有子表达式，怎依次返回各个字表达匹配到一个元素。
match，如果没有子元素，只返回所有满足条件的字符串；如果有字表达，返回包含子元素的表达式。

# 9.ajax跨域请求携带客户端cookie信息
- 开启**withCredentials**
```
let instance = axios.create({
  baseURL: config.uri.api,
  withCredentials: true
})
```
- server端，不能使用`res.header("Access-Control-Allow-Origin", "*");`，可以指定白名单，用逗号分隔，也可以动态指定，如下
```
app.all("*", function(req: any, res, next) {
  res.header(
    "Access-Control-Allow-Origin",
    // 关键代码
    req.headers.origin
  );
  res.header("Access-Control-Allow-Credentials", "true");
  res.header(
    "Access-Control-Allow-Headers",
    "Content-Type, Content-Length, Authorization, Accept, X-Requested-With , yourHeaderFeild"
  );
  res.header(
    "Access-Control-Allow-Methods",
    "PUT, POST, GET, DELETE, OPTIONS,PATCH"
  );
  if (req.method == "OPTIONS") {
    res.sendStatus(200);
    /让options请求快速返回/;
  } else {
    next();
  }
});
```

# 10.webpack devser proxy配置
```
devServer: {
    contentBase: "./", //webpack server read file path
    colors: true, //terminal shows log with color
    historyApiFallback: true, //
    progress: true,
    compress: true,
    disableHostCheck: true,
    port: 9000,
    proxy: {
      "/game-be": {
        target: "http://127.0.0.1:8888/",
        secure: false,
        pathRewrite: {"^/game-be" : ""}        
      }
    }
  },
```
说明："/game-be"指的是，/game_be/xxx的请求将代理到http://127.0.0.1:8888/。如果没有指定pathRewrite的话，
http://localhost:9000/game_be/userInfo 将会转换为 http://127.0.0.1:8888/game_be/userInfo。<br>
如果指定pathRewrite，转换后的结果将是 http://127.0.0.1:8888/userInfo 。