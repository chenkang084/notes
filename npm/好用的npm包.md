# 好用的npm包
@(nodejs)
- [chalk](https://github.com/chalk/chalk) : 可以在控制台上输出各种style,color,background。
![Alt text](../imgs/npm/7d972749-3436-40f9-b6b7-6e00eff9d8ad.png)

- [dotenv](https://github.com/motdotla/dotenv) : 在工程的根目录建立**.env**文件，可以将该文件内容读取到**process.env**对象上。
```javascript
//.env
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
//nodejs file
var db = require('db')
db.connect({
  host: process.env.DB_HOST,
  username: process.env.DB_USER,
  password: process.env.DB_PASS
})
```
- [intro.js](https://github.com/usablica/intro.js) : 主要用于网页的guide模块。
- [nprogress](https://github.com/rstacruz/nprogress) : 主要用于做ajax请求进度条。

- [keymaster](https://github.com/madrobby/keymaster) :可以对键盘各种按键进行监听。