# NodeJs的第一个程序 

```js
var http = require("http");   #引入 NodeJs自带的"http"模块
http.createServer((request, response)=>{  #创建服务器
    response.writeHead(200, {"Content-type": "text/plain"})   //设置响应头
    response.end("hello world")  #响应内容
}).listen(8800); #监听端口
```

# NPM使用

**查看版本号**:

![image-20220722141828859](D:/ProgramFiles/typora/typora-images/image-20220722141828859.png)

**使用NPM安装模块**:

```js
npm install <module name>
```

![image-20220722142033783](D:/ProgramFiles/typora/typora-images/image-20220722142033783.png)

安装的模块会放在工作目录下面的`node_modules`目录下面

使用`require("express")`导入此模块

***

**全局安装与本地安装**：

```js
npm install express # 本地安装
npm install express -g # 全局安装 
```

1. 本地安装
   * 将安装包放在`./node_modules`目录下
   * 使用`require()`命令引入本地安装的包

2. 全局安装 
   * 将安装包放在`/usr/local` 或者 node的安装目录
   * 直接在命令行里使用
   
3. 指定版本安装

   ![image-20220724112915851](D:/ProgramFiles/typora/typora-images/image-20220724112915851.png)



**查看安装信息**

使用`npm list -g`查看所有全局安装的模块

![image-20220722143015820](D:/ProgramFiles/typora/typora-images/image-20220722143015820.png)

查看某个模块的具体信息

`npm list 模块名`



**使用package.json及属性说明**

```json
{
  "name": "express",     // name 包名
  "description": "Fast, unopinionated, minimalist web framework",  // description 包的描述
  "version": "4.18.1",  // version 版本号
  "author": "TJ Holowaychuk <tj@vision-media.ca>",   // author 包的作者
  "contributors": [     // contributors  包的其他贡献者
    "Aaron Heckmann <aaron.heckmann+github@gmail.com>",
    "Ciaran Jessup <ciaranj@gmail.com>",
    "Douglas Christopher Wilson <doug@somethingdoug.com>",
    "Guillermo Rauch <rauchg@gmail.com>",
    "Jonathan Ong <me@jongleberry.com>",
    "Roman Shtylman <shtylman+expressjs@gmail.com>",
    "Young Jae Sim <hanul@hanul.me>"
  ],
  "license": "MIT",    // 开源协议
  "repository": "expressjs/express",    // 包代码存放的地方类型
  "homepage": "http://expressjs.com/",   // 主页
  "keywords": [     // 关键词
    "express",
    "framework",
    "sinatra",
    "web",
    "http",
    "rest",
    "restful",
    "router",
    "app",
    "api"
  ],
  "dependencies": {       // 依赖包。如果依赖包没有安装，npm会自动将依赖包安装在node_modules目录下
    "accepts": "~1.3.8",
    "array-flatten": "1.1.1",
    "body-parser": "1.20.0",
    "content-disposition": "0.5.4",
    "content-type": "~1.0.4",
    "cookie": "0.5.0",
    "cookie-signature": "1.0.6",
    "debug": "2.6.9",
    "depd": "2.0.0",
    "encodeurl": "~1.0.2",
    "escape-html": "~1.0.3",
    "etag": "~1.8.1",
    "finalhandler": "1.2.0",
    "fresh": "0.5.2",
    "http-errors": "2.0.0",
    "merge-descriptors": "1.0.1",
    "methods": "~1.1.2",
    "on-finished": "2.4.1",
    "parseurl": "~1.3.3",
    "path-to-regexp": "0.1.7",
    "proxy-addr": "~2.0.7",
    "qs": "6.10.3",
    "range-parser": "~1.2.1",
    "safe-buffer": "5.2.1",
    "send": "0.18.0",
    "serve-static": "1.15.0",
    "setprototypeof": "1.2.0",
    "statuses": "2.0.1",
    "type-is": "~1.6.18",
    "utils-merge": "1.0.1",
    "vary": "~1.1.2"
  },
  "devDependencies": {
    "after": "0.8.2",
    "connect-redis": "3.4.2",
    "cookie-parser": "1.4.6",
    "cookie-session": "2.0.0",
    "ejs": "3.1.7",
    "eslint": "7.32.0",
    "express-session": "1.17.2",
    "hbs": "4.2.0",
    "marked": "0.7.0",
    "method-override": "3.0.0",
    "mocha": "9.2.2",
    "morgan": "1.10.0",
    "multiparty": "4.2.3",
    "nyc": "15.1.0",
    "pbkdf2-password": "1.2.1",
    "supertest": "6.2.3",
    "vhost": "~3.0.2"
  },
  "engines": {
    "node": ">= 0.10.0"
  },
  "files": [
    "LICENSE",
    "History.md",
    "Readme.md",
    "index.js",
    "lib/"
  ],
  "scripts": {
    "lint": "eslint .",
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "nyc --reporter=lcovonly --reporter=text npm test",
    "test-cov": "nyc --reporter=html --reporter=text npm test",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
  }
}
```

**查看某个模块的所有版本**：

`npm view 模块 versions`    

**卸载模块**：

`npm uninstall 模块名`

**更新模块**：

`npm update express`

**搜索模块**：

`npm search 模块名`

**创建模块**：

```js
E:\ProcessDocuments\web-front\nodejs\src>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (src) biienu
version: (1.0.0)
description: 自定义测试Npm
entry point: (hello.js)
test command: make test
git repository: https://github.com/biienu/vue-test.git
keywords: biienu
author: biienu
license: (ISC)
About to write to E:\ProcessDocuments\web-front\nodejs\src\package.json:

{
  "name": "biienu",
  "version": "1.0.0",
  "description": "自定义测试Npm",
  "main": "hello.js",
  "devDependencies": {},
  "scripts": {
    "test": "make test"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/biienu/vue-test.git"
  },
  "keywords": [
    "biienu"
  ],
  "author": "biienu",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/biienu/vue-test/issues"
  },
  "homepage": "https://github.com/biienu/vue-test#readme"
}

Is this OK? (yes) yes
```

然后在资源库中注册用户：

```js

E:\ProcessDocuments\web-front\nodejs\src>npm adduser
npm notice Log in on https://registry.npmjs.org/
Username: biienu
Password:
Email: (this IS public) 
npm notice Please check your email for a one-time password (OTP)
Enter one-time password: 
Logged in as biienu on https://registry.npmjs.org/.
```

最后使用npm publish发布模块



查看NPM所有的命令：npm help

查看某个npm命令的使用：npm 命令 -h



# Node.js REPL(交互式解释器)

> Node.js REPL(Read Eval Print Loop)  表示一个电脑的环境，类似 Windows终端，Linux的 shell，我们可以在终端中输入命令，并接收系统的响应。
>
> Node.js 自带交互式解释器，可以执行以下任务:
>
> * 读取：读取用户输入，解析输入的JavaScript数据结构并存储在内存中。
> * 执行：执行输入的数据结构 
> * 打印：输出结果。
> * 循环：循环操作以上步骤，直到用户按两次 ctrl + c 终止循环。



启动终端

![image-20220722151348347](D:/ProgramFiles/typora/typora-images/image-20220722151348347.png)

简单的表达式计算

![image-20220722151516384](D:/ProgramFiles/typora/typora-images/image-20220722151516384.png)

使用变量
![image-20220722151701493](D:/ProgramFiles/typora/typora-images/image-20220722151701493.png)

多选表达式：

```js
> x
10
> do {
... console.log("x=" + x);
... x--;
... }while(x> 0)
x=10
x=9
x=8
x=7
x=6
x=5
x=4
x=3
x=2
x=1
1
>
```

**下划线变量**

使用  _  获取上一步的结果

![image-20220722152137490](D:/ProgramFiles/typora/typora-images/image-20220722152137490.png)

***

**REPL命令**：

![image-20220722152344688](D:/ProgramFiles/typora/typora-images/image-20220722152344688.png)



ctrl + c 退出 当前终端

ctrl + c 两次，退出Node REPL

ctrl + d 退出 Node REPL

****



# Node.js 回调函数

![image-20220722153629828](D:/ProgramFiles/typora/typora-images/image-20220722153629828.png)

***

**阻塞代码demo**：

1. 创建一个 fx.txt文件，内容如下 ：

```tex
hello, this is fx.txt content
```

2. 创建demo.js文件

```js
var readFile = require("fs"); //引入 读取文件模块
var data = readFile.readFileSync("fx.txt");
console.log("data=>  " + data.toString());
console.log("程序执行结束");
```

3. 程序执行结果

![image-20220722154051484](D:/ProgramFiles/typora/typora-images/image-20220722154051484.png)

*先读取完文件，再执行下面的程序*



**异步代码**：

demo.js 文件

```js
var readFile = require("fs"); //引入 读取文件模块
// var data = readFile.readFileSync("fx.txt");
// console.log("data=>  " + data.toString());
// console.log("程序执行结束");

readFile.readFile("fx.txt", (err, data)=>{
    if(err) return "读取文件错误";
    console.log(data.toString());
})
console.log("程序执行结束");
```



程序执行结果：

![image-20220722154408736](D:/ProgramFiles/typora/typora-images/image-20220722154408736.png)

*不等读取文件结束，就可以执行下面的程序*

***



# Node.js 事件循环

**Node.js 事件循环**：

![image-20220722155423458](D:/ProgramFiles/typora/typora-images/image-20220722155423458.png)

***

**事件驱动程序**：

![image-20220722155513459](D:/ProgramFiles/typora/typora-images/image-20220722155513459.png)

![image-20220722155551734](D:/ProgramFiles/typora/typora-images/image-20220722155551734.png)

**Demo样例**：

```js
var events = require("events");    // 引入 events 模块
var eventEmit = new events.EventEmitter();// 创建EventEmitter对象

//创建事件驱动程序
function connected() {
    console.log("连接成功");

    //触发data_received事件
    eventEmit.emit("data_received");
}
// 绑定connection事件和事件驱动程序 
eventEmit.on("connection", connected);
// 绑定data_received事件，并使用匿名函数设置驱动程序
eventEmit.on("data_received", ()=>{
    console.log("数据库连接成功");
})
//触发connection事件
eventEmit.emit("connection");
console.log("程序执行结束");
```

程序执行效果：

![image-20220722160441458](D:/ProgramFiles/typora/typora-images/image-20220722160441458.png)

****



# Node.js EventEmitter对象

https://www.runoob.com/nodejs/nodejs-event.html



***

# Buffer类

![image-20220722163019788](D:/ProgramFiles/typora/typora-images/image-20220722163019788.png)









