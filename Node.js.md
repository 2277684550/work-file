## Node.js

### 1.初识Node.js

#### 1.为什么 Javescript 可以在浏览器中运行

![image-20221213164421135](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213164421135.png)

因为浏览器自带解析引擎，可以解析js代码，其中以Chrome的 v8 引擎最好用

#### 2.为什么 JavaScript 可以操作 DOM 和 BOM

![image-20221213164757298](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213164757298.png)

![image-20221213165042945](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165042945.png)

因为浏览器自带 DOM API 、BOM API、Ajax API ,js代码可以调用浏览器自带的这些Web API，然后再通过解析引擎来解析js代码

#### 3.浏览器中 JaveScript 运行环境

![image-20221213165146588](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165146588.png)

![image-20221213165202494](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165202494.png)

运行环境必须需要 解析引擎 与 浏览器的 内置API

### 2. Node.js 简介

#### 1. 什么是 Node.js

![image-20221213165413145](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165413145.png)

#### 2. Node.js 中的 JavaScript 运行环境

![image-20221213165551190](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165551190.png)

![image-20221213165607588](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165607588.png)

因为Node.js 里面没有内置浏览器API，而是有一套自己的API

#### 3. Node.js 怎么学

![image-20221213165741803](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165741803.png)

#### 4. 在 Node.js 环境中执行 JavaScript 代码

![image-20221213165905296](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213165905296.png)

必须得处在要执行的js文件目录下再 node 才行

![image-20221213171541907](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213171541907.png)

**终端中的快捷键**

![image-20221213170036993](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213170036993.png)

### 3. fs 文件系统模块

#### 1.什么是 fs 文件系统模块

![image-20221213170600396](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213170600396.png)

#### 2.fs.readFile() 读取内容

![image-20221213171020155](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213171020155.png)

示例代码

![image-20221213174104063](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213174104063.png)

成功读取后 err 返回null

​					dataStr 返回内容

![image-20221213174309241](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213174309241.png)

读取失败后 err 返回一个错误对象

  				  dataStr 的值为 underfined

#### 3.判断文件是否读取成功

![image-20221213175227369](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213175227369.png)

修改文件路径

![image-20221213175305978](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213175305978.png)

#### 4. fs.writeFile() 写入内容

![image-20221213205100657](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213205100657.png)

 写入成功

![image-20221213210206465](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213210206465.png)

写入失败

![image-20221213210410907](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213210410907.png)

#### 5.判断文件是否写入成功

写入成功

![image-20221213211001887](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213211001887.png)

写入失败

![image-20221213210910773](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213210910773.png)

#### 6.考试成绩整理（练习）

![image-20221213211645644](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213211645644.png)

![image-20221213211621324](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213211621324.png)

04.考试成绩整理.js

```js
// 1.导入模块
const fs = require('fs')

// 2.调用fs.readFile() 读取文件内容
fs.readFile('./files/成绩.txt','utf8', function (err, data) {
    // 3.判断是否成功
  if (err) return console.log('读取失败'+err.message);
//   console.log('读取成功'+data);
    // 4.1 先把成绩的数据，按照空格进行分割
    const arrOld = data.split(' ')
    // 4.2 循环分割后的数组，对每一项数据，进行字符串的替换操作
    const arrNew = []
    arrOld.forEach(item => {
        // 4.3 每一次循环，都为新数组追加一次值，并把 = 替换成 ：
        arrNew.push(item.replace('=',':'))
    });
    // 4.4 把新数组的每一项，进行合并，得到一个新的字符串
    // 4.5 在windows中 \r\n为回车换行，这里是把数组合并成字符串每一值之间用\r\n连接
    const newStr = arrNew.join('\r\n')
    console.log(newStr);

    // 5.1调用 fs.writeFils() 方法，把处理完毕的成绩，写入到新的文件中
    fs.writeFile('./files/成绩OK.txt',newStr,function(err){
        // 5.2 如果写入成功 err 的值为一个错误对象，if(一个对象){执行}
            //  反之写入成功， err 的值为 null 便不会执行，而是会跳过 if
        if(err)console.log('成绩写入失败');
        console.log('成绩写入成功');
    })
});
```

![image-20221213213913170](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213213913170.png)

![image-20221213213924594](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213213924594.png)

#### 7.路径动态拼接问题

![image-20221213220950333](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213220950333.png)

![image-20221213220931958](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213220931958.png)

![image-20221213221005933](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213221005933.png)

![image-20221213221152965](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213221152965.png)

![image-20221213221128382](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213221128382.png)

彻底解决方案

__dirname

![image-20221213221652891](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213221652891.png)

### 4.path 路径模块

#### 1. 什么是 path 路径模块

![image-20221213221901063](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213221901063.png)

#### 2. path.join 路径拼接

![image-20221213222138893](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213222138893.png)

![image-20221213222429215](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213222429215.png)

![image-20221213222801569](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213222801569.png)

![image-20221213222840574](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213222840574.png)

![image-20221213223211435](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213223211435.png)

#### 3. path.basename 获取路径中的文件名

![image-20221213223409714](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213223409714.png)

![image-20221213223515232](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213223515232.png)

![image-20221213224336841](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213224336841.png)

#### 4.path.extname 获取文件拓展名

![image-20221213224533295](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213224533295.png)

![image-20221213224611446](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213224611446.png)

![image-20221213224956257](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213224956257.png)

#### 5.时钟（综合案例）

![image-20221213225444090](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213225444090.png)

![image-20221213225607408](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213225607408.png)

![image-20221213234629888](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221213234629888.png)

<img src="C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214105535634.png" alt="image-20221214105535634" style="zoom:80%;" />

![image-20221214105833168](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214105833168.png)

![image-20221214113112612](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214113112612.png)

<img src="C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214113013549.png" alt="image-20221214113013549" style="zoom:80%;" />

09.时钟案例.js

```js
// 1.引入模块
const fs = require('fs')
const path = require('path')

// 2.1定义正则表达式，分别匹配 <style></style> 和 <script></script>
// \s为匹配空白字符 \S为匹配非空白字符 *为任意次 而且需要个后面的标签的/ 加一个\ 转义 
const regStyle = /<style>[\s\S]*<\/style>/
const regScript = /<script>[\s\S]*<\/script>/

// 2.2 读取数据
fs.readFile(path.join(__dirname,'./时钟案例/素材/index.html'),'utf8',function(err,dataStr){
    if(err){
        return console.log('读取内容失败！');
    }
    // 读取文件成功后,调用下面对应的三个方法,分别拆解出 css,js,html文件
    resolveCSS(dataStr)
    resolveJS(dataStr)
    resolveHTML(dataStr)
})

// 3.1处理CSS样式的方法
function resolveCSS(htmlStr){
    // 3.2 使用正则提取需要的内容
        // exec方法用来检索字符串的正则表达式的匹配
        // 返回一个数组,其中存放匹配的结果,如果未找到匹配结果返回 null
        // 此时索引为0 的数组便是我们正则表达式所匹配的数组
    const r1 = regStyle.exec(htmlStr)
    // 3.3 将提取出来的字符串,进行字符串的 replace 替换操作,此时newCSS的值变为纯css代码了
    const newCSS = r1[0].replace('<style>','').replace('</style>','')
    
    // 3.4 调用fs.writeFile() 方法,将提取出来的样式, 写入到 时钟案例/clock目录中 index.css 的文件里面
    fs.writeFile(path.join(__dirname,'./时钟案例/clock/index.css'),newCSS,function(err){
        if(err){
            console.log('index.js 写入失败!'+err.message);
        }
        console.log('index.css 写入成功!');
    })
    
}

// 4.1处理JS样式的方法
function resolveJS(htmlStr){
    // 4.2 使用正则提取需要的内容
        // exec方法用来检索字符串的正则表达式的匹配
        // 返回一个数组,其中存放匹配的结果,如果未找到匹配结果返回 null
        // 此时索引为0 的数组便是我们正则表达式所匹配的数组
    const r2 = regScript.exec(htmlStr)
    // 4.3 将提取出来的字符串,进行字符串的 replace 替换操作,此时newCSS的值变为纯css代码了
    const newJS = r2[0].replace('<script>','').replace('</script>','')
    
    // 4.4 调用fs.writeFile() 方法,将提取出来的样式, 写入到 时钟案例/clock目录中 index.css 的文件里面
    fs.writeFile(path.join(__dirname,'./时钟案例/clock/index.js'),newJS,function(err){
        if(err){
            console.log('index.js 写入失败!'+err.message);
        }
        console.log('index.js 写入成功!');
    })
    
}

// 5.1 处理 HTML 结果的方法
function resolveHTML(htmlStr){
    // 5.2 将字符串调用 replace 方法,把内嵌的 style 和 script ,替换为外联的 link 和 script标签
        // 先用regStyle正则表达式匹配到htmlStr里面的内容,然后用后面的外联的内容替换掉匹配的内容,script同理
    const newHTML = htmlStr.replace(regStyle,'<link rel="stylesheet" href="./index.css">').replace(regScript,'<script src="./index.js"></script>')
    // 5.3 写入 index.html 这个文件
    fs.writeFile(path.join(__dirname,'/时钟案例/clock/index.html'),newHTML,function(err){
        if(err){
            console.log('index.html 写入失败!'+err.message);
        }
        console.log('index.html 写入成功!');
    })
}
```

![image-20221214165941582](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214165941582.png)

![image-20221214170400137](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214170400137.png)

### 5.http 模块

#### 1.什么是 http 模块

![image-20221214170653104](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214170653104.png)

#### 2.进一步理解 http 模块的作用

<img src="C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214172647922.png" alt="image-20221214172647922" style="zoom:150%;" />

#### 3.服务器相关概念

##### 1.IP地址

![image-20221214173555541](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214173555541.png)

##### 2.域名和域名服务器

![image-20221214174928888](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214174928888.png)

##### 3.端口号

![image-20221214175545584](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214175545584.png)

#### 4.创建最基本的 web 服务

**![image-20221214175844604](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214175844604.png)**

​					![image-20221214175904869](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214175904869.png)

![image-20221214175929032](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214175929032.png)



​					**![image-20221214180046053](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214180046053.png)**

![image-20221214180227801](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214180227801.png)

node.js\http 模块\01.创建基本的web服务器.js

```js
// 1. 导入 http 模块
const http = require('http')

// 2. 创建 web 服务器实例
const server = http.createServer()
// 3. 为服务器实例绑定 request 事件，监听客户端的请求
    // 只要有人通过4的ip地址访问，就会触发request事件
server.on('request',function(req,res){
    console.log('Someone visit our web server');
})
// 4. 启动服务器
    // 如果监听的是80端口，可以不用在ip后面添加端口号，
    // 如果不是80端口，需要在ip后面补全端口
server.listen(80,function(){
    console.log('server running at http://127.0.0.1');
})
```

![image-20221214183201441](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214183201441.png)

**![image-20221214183516993](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214183516993.png)**

02.了解req请求对象.js

```js
const http = require('http')

const server = http.createServer()

//req 是请求对象，包含了与客户端相关的数据和属性
server.on('request',(req) => {
    // req.url 是客户端的请求的 URL 地址
    const url = req.url

    // req.method 是客户端请求的 method 类型
    const method = req.method

    const str = `Your request url is ${url}, and request method is ${method}`
    console.log(str);
})
server.listen(80,() => {
    console.log('server running at http://127.0.0.1');
})
```

![image-20221214185432212](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214185432212.png)

![image-20221214185639393](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214185639393.png)

![image-20221214190415318](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214190415318.png)

![image-20221214190539286](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214190539286.png)

此时浏览器请求ip便会输出你想要响应的内容



![image-20221214190501324](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214190501324.png)

![image-20221214190610706](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214190610706.png)

![ image-20221214191131085](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214191131085.png)

此时便不乱码了

```js
// 调用 res.setHeader() 方法，设置 COntent-Type 响应头，解决中文乱码的问题
    res.setHeader('Content-Type','text/html; charset=utf-8')
```

#### ![image-20221214191456448](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221214191456448.png)5. 根据不同的 url 响应不同的 html内容

![image-20221215024031317](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215024031317.png)

![image-20221215024055016](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215024055016.png)

http 模块/04.根据不同的url响应不同的html内容.js

```js
const http = require('http')
// 创建一个 Web 服务器
const server = http.createServer()

server.on('request',(req,res) => {

    // 1.获取请求的 URL 地址
    const url = req.url
    // 2.设置默认的响应内容为 404 NOt found
    let content ='<h1>404 NOt found</h1>'
    // 3.判断用户的的请求是否为 / 或 /index.html   首页
    if(url === '/' ||url ==='/index.html'){
        content = '<h1>首页</h1>'
    }
    // 4.判断用户的的请求是否为 /about.html   关于页面
    if(url ==='/about.html'){
        content = '<h1>关于页面</h1>'
    }
    // 5.设置 Content-Type 响应头，防止中文乱码
    res.setHeader('Content-Type','text/html;charset=utf8')
    // 6.使用 res.end() 把内容响应给客户端
    res.end(content)
})

server.listen(80,() => {
    console.log('server running at http://127.0.0.1');
})
```

![image-20221215025419126](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215025419126.png)

#### 6.	实现 clock 时钟的 web 服务器（案例）

​					![image-20221215025530571](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215025530571.png)

​					![image-20221215025553015](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215025553015.png)

![image-20221215025628973](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215025628973.png)

![image-20221215025635475](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215025635475.png)

![image-20221215025703453](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215025703453.png)

![image-20221215030443466](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215030443466.png)

![image-20221215034115416](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215034115416.png)

05.clock时钟web服务器.js

```js
const http = require('http')
const fs = require('fs')
const path = require('path')

// 创建web服务器
const server = http.createServer()

server.on('request',(req,res) => {
    // 1.1获取到客户端的 URL 地址
    const url = req.url
    // 1.2把请求的 URl 地址映射为具体的 URL 地址
        // 因为获取到的地址不是完整的，我们需要完整的路径，所以需要在前面加上该文件所处的目录
    const fpath = path.join(__dirname,url)

    // 如果使用请求头的话css样式会不起效果
    // res.setHeader('Content-Type','text/html;chaset=utf8')
    
    // 2.1 根据'映射'过来的文件路径读取文件的内容
    fs.readFile(fpath,'utf8',(err,dataStr) => {
        //2.2 如果读取失败，向客户端响应固定的'错误消息'
        if(err){
            return res.end('404 Not found')
        }
        // 2.3 读取成功，就把读取成功的内容响应给客户端
        res.end(dataStr)
    })
})

server.listen(80,() => {
    console.log('server running at http://127.0.0.1');
})
```

但是此时页面显示 404 

![image-20221215033845090](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215033845090.png)

这是因为req.url拿到的地址为 / ,而该js文件所处的启动目录下面没有 / 文件,所以找不到报错

我们只需要在地址后面加上  127.0.0.1/test/index.html 就可以获取了

![image-20221215034050389](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215034050389.png)

为什么只指向了index.html，却可以获取它的样式和js事件呢，这是因为![image-20221215034228998](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215034228998.png)

目录下有index.js和index.js文件，index.html的link与src都可以找到文件，所以可以获取

![image-20221215034334951](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215034334951.png)

#### 7.优化资源的请求路径

![image-20221215034645149](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215034645149.png)

![image-20221215035318783](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215035318783.png)

此时打开页面便可以之间访问了

![image-20221215035343557](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215035343557.png)

### 6.Node.js 中的模块化

#### 1.模块的基本概念

##### 1. 什么是模块化

![image-20221215192132579](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192132579.png)

![image-20221215192310101](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192310101.png)

##### 2. 编程领域的模块化

![image-20221215192338610](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192338610.png)

##### 3.	模块化的规范

![image-20221215192422356](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192422356.png)

##### 4. Node.js 中模块的分类

![image-20221215192506230](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192506230.png)

##### 5. 加载模块

![image-20221215192551092](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192551092.png)

![image-20221215192950748](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215192950748.png)

#### 2. 模块化作用域

##### 1. 什么是模块化作用域

![image-20221215193117301](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215193117301.png)

##### 2.模块化作用域的优点

![image-20221215193354332](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215193354332.png)

![image-20221215194043543](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215194043543.png)

##### 3.向外共享模块作用域中的成员

###### 1. modl 

![image-20221215194237803](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215194237803.png)

![image-20221215194505006](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215194505006.png)

###### 2. module.exports()方法

![image-20221215195113031](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215195113031.png)

![image-20221215195413336](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215195413336.png)

###### 3. 共享成员时的注意点

![image-20221215200106363](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215200106363.png)

![image-20221215200052275](C:\Users\22776\AppData\Roaming\Typora\typora-user-images\image-20221215200052275.png)

###### 4. exports 对象

