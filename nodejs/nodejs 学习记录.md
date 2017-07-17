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