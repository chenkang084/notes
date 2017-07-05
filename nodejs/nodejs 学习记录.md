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