创建完项目，并清空多余文件和代码

# 1.项目准备

## 1.路由Home.vue

Home.vue

```vue
<template>
  <div class="home">
   我是Home
  </div>
</template>

<script>

export default {
  name: 'Home',
  components: {
   
  }
}
</script>

```

router/index.js

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component:() => import('@/views/Home.vue')
  },
]

const router = new VueRouter({
  routes
})

export default router

```

在App.vue中使用路由分配符，使进入主页面时，自动路由到path为 / 的页面

```vue
<template>
  <div id="app">
    <!-- 路由分配符 -->
   <router-view></router-view>
  </div>
</template>
```

此时页面显示为

![image-20221218222542885](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218222542885.png)

## 2.封装axios请求

### 1 . 创建全局配置js文件

![image-20221219000907940](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219000907940.png)

```js
export default {
    title:'lcelm', //项目的名称
    baseUrl:{
        dev:'http://localhost:3000', //开发环境后台接口地址
        pro:"" //生产环境，上线产品发布之后，后台接口地址
    }
}
```

## 

### 2. 创建要封装的axios.js文件

​	没有axios的话可以在vue ui中安装axio依赖

![image-20221218223159478](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218223159478.png)

![image-20221218223026341](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218223026341.png)

```js
// 1.1 引入axios
import axios from 'axios'
// 1.2 引入全局配置文件
import config from '@/config/index'

// 2. 获取开发环境，process.env.NODE_ENV 全局对象获取当前的node 的开发环境，如果是development模式，则使用config的开发环境地址，否则就是生产环境地址
const baseUrl = process.env.NODE_ENV === 'development' ? config.baseUrl.dev :config.baseUrl.pro

// 3. 定义一个类
class HttpRequest{
    //3.1. 定义一个构造函数,配置我们请求的Url地址
    constructor(baseUrl){
        this.baseUrl = baseUrl //设置Url地址
        this.queue = {} //对请求队列进行处理
    }
     //3.2.1 axios配置
     getInsideConfig(){
        // 3.2.2 要定死的配置
        const config = {
            baseURL:this.baseUrl,
            // 全局要配置的请求头
            header:{
                //
            }
        }
        // 3.2.3 返回配置结果
        return config
    }
    //3.3.1 请求拦截,把实例（instance请求）引入进来拦截，处理一下
    interceptors(instance,url){
        // 3.3.2 拦截请求，interceptors为Axios拦截器，request为请求拦截
        instance.interceptors.request.use((config) => {
              //3.3.2.1 处理config
            console.log('拦截和处理请求');
            config.data = {
                msg:"HelloWord"
            }
            console.log(config);
            // 3.3.2.2 返回config
            return config
        })
        // 3.3.3 拦截响应，对后台返回数据进行拦截，response为响应拦截
        instance.interceptors.response.use((res) => {
            //3.3.3.1处理响应
            console.log('拦截和处理响应');
            console.log(res);
            // 3.3.3.2 返回响应数据的data数据
            return res.data
        },(error) => { // 3.3.3.3 如果请求失败要执行的函数
            // 请求出问题，处理问题
            console.log(error);
            // 3.3.3.4返回处理的操作
            return {error:'网络出错了'}
        })
    }
    // 4.1真正的发起请求，options为自己的配置，要与getInsideConfig()的config 配置结合
    request(options){
        // 4.2 用axios.create()创造一个真正的axios实例对象
        const instance = axios.create() 
        // 4.3 Object.assign() 将getInsideConfig()返回的配置与options结合，属性相同的部分options会覆盖掉getInsideConfig()的config
        // 因为我们的getInsideConfig()的config只定义了请求地址和请求头，而options为我们自己的配置，用自己的配置覆盖掉config的配置
           // Object.assign()的用法  1、源对象属性替换目标对象属性  2、同名属性，后面替换前面属性  3、基本数据类型字符串生成对象
        options = Object.assign(this.getInsideConfig(),options)
        // 4.4 对请求进行劫持，也就是调用上面的请求拦截函数，第一个参数为实例化的axios请求，第二个参数为我们自己配置的url地址
        this.interceptors(instance,options.url)
        // 4.5 此时4.4已经完成了对请求的拦截和处理，我们需要把处理过的instance请求，return出去，使其可以继续发送请求，
            而且里面要携带我们自己的配置
        return instance(options) 
    }
}

// 4. 把步骤3定义的类给实例化，可以得到实例化的对象，并传入经过步骤2判断的URL地址
const axiosObj = new HttpRequest(baseUrl)

// 5. 把这个实例化的对象给暴露出去
export default axiosObj
```

### 3. 创建所有axios请求js文件

![image-20221218223722934](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218223722934.png)

```js
// 引入axios
import axios from '../api/axios'

// 导出我们定义好的（比如轮播图）箭头函数
export const getBannerData = () => {
    // 返回我们用axios去请求的内容，axios里面现在有我们封装好的request方法，可以实例化instance对象，
    // instance对象把我们自己定义的配置进行导入，然后覆盖掉原本的默认配置
    return axios.request({
        url:'banner',//请求地址
        methods:'get'//请求方法
    })
}
```

## 3.创建服务器

### 1. 在项目的同级目录下创建文件夹并创建index.js文件

![image-20221218224027939](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218224027939.png)

### 2. 安装express  

`cnpm install express -- save`

![image-20221218224222283](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218224222283.png)

### 3. 允许跨域

​	`npm install cors`

![image-20221218224340118](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218224340118.png)

### 4. 安装mysql

` npm install mysql`

![image-20221218233416652](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218233416652.png)

### 5. 创建服务

安装解析前端传值的包

`npm install body-parser `

server_hardware/index.js

```js
// 1.引入express框架
let express = require('express')
// 2.用express实例化一个app对象
let app = express()

// 3.允许跨域
const cors = require('cors')
app.use(cors()) 

// 4.1 导入解析前端传入进来的值的包
var bodyParser = require('body-parser');

// 4.2 解析 application/json
app.use(bodyParser.json());
// 4.3 解析 url编码
app.use(bodyParser.urlencoded({ extended: true }));

// 4.4 配置表单数据的中间件，注意：这个中间件只能解析 application/x-www-form-urlencoded 格式的表单数据
app.use(express.urlencoded({ extended:false}))

// 5.在全局定义中间件
// 一定要在路由之前，封装 res.cc 函数
app.use((req,res,next) => {
    // status 默认值为1 表示失败的情况
    // err的值可能是一个错误对象，也可能时一个错误的描述字符串
    res.cc = function (err,status = 1){
        res.send({
            status,
            // 三元表达式，如果调用了 ERR 错误返回函数，那么就返回错误对象，否则就是没有调用错误函数，说明传入的是一个错误字符串
            message:err instanceof Error ? err.message :err
        })
    }
    // express 中间件函数。调用next() 基本上会转移到下一个中​​间件函数
    next()
})

// 6.导入并使用路由模块
const userRouter = require('./router/user')
// 每次访问路由模块的每一个路由的时候都必须在地址前面加上一个api
app.use('/api',userRouter)



// 7.监听刚才写的3000端口，并回调输出地址
app.listen(3000,() => {
    console.log('http://localhost:3000');
})
```

### 6. 连接服务器

![image-20221219164151234](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219164151234.png)

```js
//引入mysql模块
let mysql = require("mysql")

// 连接数据库 mysql.createConnection方法是连接数据库，把它实例化，里面需要传入一些参数
let db = mysql.createConnection({
    host:'localhost',
    user:'root',
    password:'123456',
    database:'hardware'
})
// mysql.createConnection 有一个connect方法，用来连接数据库
db.connect((err) => {
    if(err){
        console.log('连接失败：'+err.stack);
    }
    console.log("连接成功");
})

module.exports = db
```



### 7. 创建路由规则文件

 该模块只负责处理路由之间的规则

![image-20221219163824756](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219163824756.png)

```js
// 该模块只负责处理路由之间的规则

const express = require('express')
// 创建路由对象
const router = express.Router()

// 导入用户路由处理函数对应的模块
const user_handler = require('../router_handler/user')

//注册新用户
        // 把客户的的请求映射到处理函数中，但是这个函数如何定义，路由模块不关心，你需要去路由处理函数去查看
router.post('/reguser',user_handler.regUser) 
//登录
router.post('/login',user_handler.login)

module.exports = router
```

### 8. 创建路由处理函数

//该模块只负责路由的处理函数

![image-20221219163930339](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219163930339.png)

安装密码加密包

`npm i bcryptjs`

![image-20221219164441356](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219164441356.png)

```js
//该模块只负责路由的处理函数
// 导入数据库操作模块
const db = require('../db/index')
// 导入 bcrypt.js 包,用来加密密码
const bcrypt = require('bcryptjs')

// 注册新用户的处理函数
exports.regUser = (req,res) => {
    // 1.获取客户端提交的表单数据
    const userinfo = req.body
    // 2.对表单的数据进行合法的判断
    if(!userinfo.username || !userinfo.password){
        return res.send({ status:1 ,message: '用户名或密码不合法'})
    }
    // 3.检测用户名是否被占用
        // 3.1 定义 SQL 语句，查询用户名是否被占用
        const salStr = 'select * from ev_users where username=?' //查询ev_users 表中的username是否有该数据，如果有，则返回一个数组
        // 3.2 调用数据库函数
        db.query(salStr,userinfo.username,(err,result) => {
            // 3.2.1 查询失败操作
            if(err){
                // 如果失败，则返回一个失败数据，状态为1则失败，为0则成功
                // return res.send({status:1,message:err.message})
                return res.cc(err)
            }
            // 3.2.2 查询成功操作
            // 判断用户名是否被占用，因为查询结果是一个数组，如果数组不为空，那么长度就不为0，说明查询到了数据，证明数据库里面有该用户名
            if(result.length > 0){
                // return res.send({ status: 1, message:'用户名被占用，请更换其他用户名'})
                return res.cc('用户名被占用，请更换其他用户名')
            }
            // 3.2.3 如果用户名可以使用，那么就加密密码
            // 调用 bcrypt.hasnSync() 对密码进行加密
            userinfo.password = bcrypt.hashSync(userinfo.password,10)
            // 3.2.4 定义插入新用户的 SQL 语句 ,通过set 快速插入一个新用户，? 为占位符
            const sql = 'insert into ev_users set ?'
            // 3.2.5 调用 db.query() 执行 SQL 语句
            db.query(sql,{username:userinfo.username,password:userinfo.password},(err,results) => {
                // 判断 SQL 语句是否执行成功
                // if(err) return res.send({ status: 1, message: err.message})
                if(err) return res.cc(err)
                // 判断影响函数是否为 1，也就是判断是否只影响了数据库的一行数据，因为插入新用户也就是插入一行，所以只影响了一行，
                // 如果不为 1，则说明插入出了问题
                // if(results.affectedRows !== 1) return res.send({ status: 1, message:'注册用户失败，请稍后重试'})
                if(results.affectedRows !== 1) return res.cc('注册用户失败，请稍后重试')
                // 注册用户成功
                // res.send({status:0,message:'注册成功 '})
                res.cc('注册成功',0)
            })
       
        })
}
```

### 9. 后端验证规则

因为if else判断对于用户名和密码来说过于简单，所以需要引入第三方包来增加验证规则

#### 1.注释掉路由处理函数中的if判断语句

![image-20221219172542060](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219172542060.png)

#### 2.下载第三方验证包

1. `npm i joi`

    ![image-20221219172814700](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219172814700.png)

2. `npm i @escook/express-joi`

    ![image-20221219172756417](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219172756417.png)

#### 3.创建验证规则文件

![image-20221219172851896](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219172851896.png)

```js
// 导入定义验证规则的包
const joi = require('joi')

// 定义用户名和密码的验证规则
// string() 值必须是字符串
// alphanum() 值只能是包含 a-zA-Z0-9 的字符串
// min(length) 最小长度
// max(length) 最大长度
// required() 值是必须项，不能为 undefined
// pattern(正则表达式) 值必须符合正则表达式的规则
const username = joi.string().alphanum().min(1).max(10).required()
const password = joi.string().pattern(/^[\S]{6,12}$/).required()

// 定义验证规则和登录表单数据的规则对象
exports.reg_login_schema = {
    // 因为我们的表单数据都在req的body里面存着，所以我们就验证body里面的数据
    // 所以我们定义一个body属性，指向这个body对象，里面包含了这两条验证规则
    body:{
        username,
        password,
    }
}

```



#### 4.在路由规则文件中处理验证

![image-20221219173123561](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219173123561.png)

```js
// 该模块只负责处理路由之间的规则

const express = require('express')
// 创建路由对象
const router = express.Router()

// 导入用户路由处理函数对应的模块
const user_handler = require('../router_handler/user')

// 1.导入验证数据的中间件
const expressJoi = require('@escook/express-joi')
// 2.导入需要的验证规则对象
    // 因为'../schema/user' 模块是一个对象,需要用一个对象来接受，又因为我们只需要里面的reg_login_schema对象，
    // 所以需要解构赋值，用reg_login_schema来接受
const {reg_login_schema} = require('../schema/user')

//注册新用户
        // 把客户的的请求映射到处理函数中，但是这个函数如何定义，路由模块不关心，你需要去路由处理函数去查看
        //加入一个参数，调用exprossJoi中间件函数，并将对应的规则传进去
        // 如果中间件的验证成功了，那就继续执行后面的处理函数
        // 如果验证失败了，将会抛出一个全局的错误，所以需要在index.js里面去捕获这个错误
router.post('/reguser',expressJoi(reg_login_schema),user_handler.regUser) 
//登录
router.post('/login',user_handler.login)

module.exports = router
```



#### 5. 在全局文件里面捕获错误

注意：在express 中不能连续使用两次res.send()

![image-20221219173225413](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219173225413.png)

![image-20221219173535767](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221219173535767.png)

### 10.登录api

#### 1.设置登陆路由

![image-20221220145038588](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220145038588.png)

#### 2.配置登录路由处理函数

![image-20221220151323461](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220151323461.png)

router_handler/user.js

```js
//该模块只负责路由的处理函数
// 导入数据库操作模块
const db = require('../db/index')
// 导入 bcrypt.js 包,用来加密密码
const bcrypt = require('bcryptjs')
//导入生成token包
const jwt = require('jsonwebtoken')
//导入全局的配置文件
const config = require('../config')
const { jwtSecretKey } = require('../config')
// 注册新用户的处理函数
exports.regUser = (req,res) => {
    // 1.获取客户端提交的表单数据
    const userinfo = req.body
    // 2.对表单的数据进行合法的判断
    // if(!userinfo.username || !userinfo.password){
    //     return res.send({ status:1 ,message: '用户名或密码不合法'})
    // }
    // 3.检测用户名是否被占用
        // 3.1 定义 SQL 语句，查询用户名是否被占用
        const salStr = 'select * from ev_users where username=?' //查询ev_users 表中的username是否有该数据，如果有，则返回一个数组
        // 3.2 调用数据库函数
        db.query(salStr,userinfo.username,(err,result) => {
            // 3.2.1 查询失败操作
            if(err){
                // 如果失败，则返回一个失败数据，状态为1则失败，为0则成功
                // return res.send({status:1,message:err.message})
                return res.cc(err)
            }
            // 3.2.2 查询成功操作
            // 判断用户名是否被占用，因为查询结果是一个数组，如果数组不为空，那么长度就不为0，说明查询到了数据，证明数据库里面有该用户名
            if(result.length > 0){
                // return res.send({ status: 1, message:'用户名被占用，请更换其他用户名'})
                return res.cc('用户名被占用，请更换其他用户名')
            }
            // 3.2.3 如果用户名可以使用，那么就加密密码
            // 调用 bcrypt.hasnSync() 对密码进行加密
            userinfo.password = bcrypt.hashSync(userinfo.password,10)
            // 3.2.4 定义插入新用户的 SQL 语句 ,通过set 快速插入一个新用户，? 为占位符
            const sql = 'insert into ev_users set ?'
            // 3.2.5 调用 db.query() 执行 SQL 语句
            db.query(sql,{username:userinfo.username,password:userinfo.password},(err,results) => {
                // 判断 SQL 语句是否执行成功
                // if(err) return res.send({ status: 1, message: err.message})
                if(err) return res.cc(err)
                // 判断影响函数是否为 1，也就是判断是否只影响了数据库的一行数据，因为插入新用户也就是插入一行，所以只影响了一行，
                // 如果不为 1，则说明插入出了问题
                // if(results.affectedRows !== 1) return res.send({ status: 1, message:'注册用户失败，请稍后重试'})
                if(results.affectedRows !== 1) return res.cc('注册用户失败，请稍后重试')
                // 注册用户成功
                // res.send({status:0,message:'注册成功 '})
                res.cc('注册成功',0)
            })
       
        })
}

// 登录的处理函数
exports.login = (req,res) => {
    // 接收表单数据
    const userInfo = req.body
    // 定义 SQL 语句
    const sql = `select * from ev_users where username=?`
    // 执行 SQL 语句，根据用户名查询用户信息
    db.query(sql,userInfo.username,(err,results) => {
        // 执行 SQL 语句失败
        if(err)return res.cc(err)
        // 执行 SQL 语句成功，但是获取到的数据条数不等于1，也就是不等于1，
            // result获取到的数据是一个数组，因为我们只查询到了一行数据，放在了数组的 0 索引处，数组的长度为 1
            // 所以如果数组的长度不为 1，那么数组就为空，说明没有查询到用户
        if(results.length !== 1) return res.cc('登录失败！')      
        // 判断密码是否正确
        // bcrypt.compareSync第一个参数为用户提交的密码，第二个参数为数据库的密码，如果相符，就返回true，否则false
        const compareResult = bcrypt.compareSync(userInfo.password,results[0].password)
        // 如果compareResult 为false，就说明密码不对
        if(!compareResult) return res.cc('登录失败')
        // 在服务器端生成 Token字符串
            // 由于密码过于敏感不能携带在token中，头像太大，也不宜放在token中，所以需要剔除
            // 用es6的运算展开符，展开这个数组，并将password与user_pic:的值都赋空
        const user = {...results[0],password:'',user_pic:''}
        // 对用户的信息进行加密，生成 Token 字符串
            // jwt.sign为taoken包的方法
            // 第一个参数为要加密的对象，第二个参数为加密的时候需要用到密钥值，第三个参数为token的有效期
        const tokenStr = jwt.sign(user,config.jwtSecretKey,{ expiresIn: config.expiresIn }) 
        // 调用 res.send 方法将 Token 响应给客户
        res.send({
            status:0,   
            message:'登陆成功',
            token: 'Bearer ' + tokenStr, //'Bearer ' 是客户端需要的，这里写上方便客户端使用，记得Bearer后面有个空格，不要漏了
        })
    })

}
```



### 11. 设置 Token

#### 1.下载Token包

生成字符串包

` npm i jsonwebtoken  `

![image-20221220110004911](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220110004911.png)

解析Token的中间件

`npm i express-jwt@5.3.3`

![image-20221220110044452](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220110044452.png)

#### 2. 创建全局配置文件 config.js

![image-20221220105337719](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220105337719.png)

#### 3. 在router_handler/user.js里面配置Token

![image-20221220105545303](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220105545303.png)

![image-20221220105638327](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220105638327.png)

```js
		/ 在服务器端生成 Token字符串
            // 由于密码过于敏感不能携带在token中，头像太大，也不宜放在token中，所以需要剔除
            // 用es6的运算展开符，展开这个数组，并将password与user_pic:的值都赋空
        const user = {...results[0],password:'',user_pic:''}
        // 对用户的信息进行加密，生成 Token 字符串
            // jwt.sign为taoken包的方法
            // 第一个参数为要加密的对象，第二个参数为加密的时候需要用到密钥值，第三个参数为token的有效期
        const tokenStr = jwt.sign(user,config.jwtSecretKey,{ expiresIn: config.expiresIn }) 
        // 调用 res.send 方法将 Token 响应给客户
        res.send({
            status:0,   
            message:'登陆成功',
            token: 'Bearer ' + tokenStr, //'Bearer ' 是客户端需要的，这里写上方便客户端使用，记得Bearer后面有个空格，不要漏了
        })
```

#### 4. 在index.js 里面配置解析 Token的中间件

此时除了以api为地址的请求不需要身份验证，其他的接口必须要有token携带在请求头里面才能请求成功

![image-20221220110354775](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220110354775.png)

### 12. 获取用户信息api

#### 1.设置请求用户信息路由

创建一个新的文件

因为是一个新的请求模块，之前是登录注册路由模块，这个是请求用户信息的模块，单独用一个文件，这样方便维护

![image-20221220153835233](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220153835233.png)

#### 2.配置请求用户信息路由处理函数

也是重写创建一个新的文件

![image-20221220161141107](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220161141107.png)

```js
// 导入数据库操作模块
const db = require('../db/index')

exports.getUserInfo = (req,res) => {
    // 定义 SQL 语句
        // where id=? 是查询id为（你要查询的那一行的数据的id）
    const sql = "select id,username,nickname,email,user_pic,balance from ev_users where id=?"
    // 调用 db.query() 执行 SQL 语句
        // 因为数据库的属性的Id，首字母是大写的，登陆时储存在token里面，现在们以token的 Id值为索引获取对应的值
    db.query(sql,req.user.Id,(err,results) => {
        if(err) return res.cc(err)
        //执行 SQL 语句成功，但是查询结果可能为空
        if(results.length !==1 ) return res.cc('获取用户信息失败')
        res.send({
            status:0,
            message:'获取用户信息成功！',
            data:results[0]
        })
    })
}
```

#### 3.在index.js中导入并使用路由模块

![image-20221220161510487](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221220161510487.png)

### 13.分别设置更新用户头像/昵称/邮箱/余额的api

#### 1.设置更新信息验证规则

schema/user.js

```js
// 导入定义验证规则的包
const joi = require('joi')

// 定义用户名和密码的验证规则
// string() 值必须是字符串
// alphanum() 值只能是包含 a-zA-Z0-9 的字符串
// min(length) 最小长度
// max(length) 最大长度
// required() 值是必须项，不能为 undefined
// pattern(正则表达式) 值必须符合正则表达式的规则
// dataUri() 值必须是bsae64 格式字符串（转成bsae64 格式的图片），如
//                                         data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAVjAAAAAA==
// email() 值必须为邮箱
// number() 值必须为数字
// integer() 值必须为整数

// 定义验证 username 用户名的验证规则
const username = joi.string().alphanum().min(1).max(10).required()
// 定义验证 password 密码的验证规则
const password = joi.string().pattern(/^[\S]{6,12}$/).required()

// 定义验证 avatar 头像的验证规则
const avatar = joi.string().dataUri().required()
// 定义验证 nickname 昵称的验证规则
const nickname = joi.string().required()
// 定义验证 email 邮箱的验证规则
const email = joi.string().email().required()
// 定义验证 balance 余额的验证规则
const balance = joi.number().integer().min(1).required()

// 定义验证规则和登录表单数据的规则对象
exports.reg_login_schema = {
    // 因为我们的表单数据都在req的body里面存着，所以我们就验证body里面的数据
    // 所以我们定义一个body属性，指向这个body对象，里面包含了这两条验证规则
    body:{
        username,
        password,
    }
}

// 验证规则对象 --更新用户头像
exports.update_avatar_schema = {
    body:{
        avatar
    }
}

// 验证规则对象 --更新用户昵称
exports.update_nickname_schema = {
    body:{
        nickname,
    }
}

// 验证规则对象 --更新用户邮箱
exports.update_email_schema = {
    body:{
        email
    }
}

// 验证规则对象 --更新用户余额
exports.update_balance_schema = {
    body:{
       balance
    }
}
```



#### 2.设置更新信息路由

router/userinfo.js

```js
const express = require('express')
const router = express.Router()

// 导入路由处理模块
const userinfo_handler = require('../router_handler/userinfo')

//导入验证数据的中间件
const expressJoi = require('@escook/express-joi')
// 导入需要的验证规则
const {update_avatar_schema,update_nickname_schema,update_email_schema,update_balance_schema} = require('../schema/user')

// 请求用户信息路由
router.get('/userinfo',userinfo_handler.getUserInfo)

// 更新用户头像路由，并引入验证规则，不通过验证规则，无法进入处理函数
router.post('/update/avatar',expressJoi(update_avatar_schema),userinfo_handler.updateAvatar)
//更新用户昵称路由，并引入验证规则，不通过验证规则，无法进入处理函数
router.post('/update/nickname',expressJoi(update_nickname_schema),userinfo_handler.updateNickname)
//更新用户邮箱路由，并引入验证规则，不通过验证规则，无法进入处理函数
router.post('/update/email',expressJoi(update_email_schema),userinfo_handler.updateEmail)
//更新用户余额路由，并引入验证规则，不通过验证规则，无法进入处理函数
router.post('/update/balance',expressJoi(update_balance_schema),userinfo_handler.updateBalance)

module.exports = router
```

#### 3.设置更新信息路由处理函数

router_handler/userinfo.js

```js
// 导入数据库操作模块
const db = require('../db/index')

// 获取用户信息
exports.getUserInfo = (req,res) => {
    // 定义 SQL 语句
        // where id=? 是查询id为（你要查询的那一行的数据的id）
    const sql = "select id,username,nickname,email,user_pic,balance from ev_users where id=?"
    // 调用 db.query() 执行 SQL 语句
        // 因为数据库的属性的Id，首字母是大写的，登陆时储存在token里面，现在们以token的 Id值为索引获取对应的值
    db.query(sql,req.user.Id,(err,results) => {
        if(err) return res.cc(err)
        //执行 SQL 语句成功，但是查询结果可能为空
        if(results.length !==1 ) return res.cc('获取用户信息失败')
        res.send({
            status:0,
            message:'获取用户信息成功！',
            data:results[0]
        })
    })
}

// 更新用户头像
exports.updateAvatar = (req,res) => {
    // 通过Id来进行定位更改信息
   const sql = `update ev_users set user_pic=? where id=?`
   db.query(sql,[req.body.avatar,req.user.Id],(err,results) => {
    if(err) return res.cc('更换头像失败')
    if(results.affectedRows !== 1) return res.cc('影响多行')
    res.cc('更换头像成功',0)
   })
}

//  更新用户昵称
exports.updateNickname = (req,res) => {
     // 通过Id来进行定位更改信息
    const sql = `update ev_users set nickname=? where id=?`
    db.query(sql,[req.body.nickname,req.user.Id],(err,results) => {
     if(err) return res.cc('更新昵称失败')
     if(results.affectedRows !== 1) return res.cc('影响多行')
     res.cc('更新昵称成功',0)
    })
 }
 //  更新用户邮箱
exports.updateEmail = (req,res) => {
     // 通过Id来进行定位更改信息
    const sql = `update ev_users set email=? where id=?`
    db.query(sql,[req.body.email,req.user.Id],(err,results) => {
     if(err) return res.cc('更新用邮箱失败')
     if(results.affectedRows !== 1) return res.cc('影响多行')
     res.cc('更新邮箱成功',0)
    })
 }

 //  更新用户余额
exports.updateBalance = (req,res) => {
     // 通过Id来进行定位更改信息
    const sql = `update ev_users set balance=balance+?  where id=?`
    db.query(sql,[req.body.balance,req.user.Id],(err,results) => {
     if(err) return res.cc('充值失败')
     if(results.affectedRows !== 1) return res.cc('影响多行')
     res.cc('充值成功',0)
    })
 }
 

```



### 14.启动服务器

`node index.js`

![image-20221218233656935](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218233656935.png)

这种方法有时候并不稳定，会断开服务器连接，我们可以用pm2管理器来让服务器一直后台保持开启状态，即便关闭终端服务器依然会运行

脚本就是服务器的启动文件，这里指index.js

![image-20221225213642857](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221225213642857.png)

## 2.搭建框架

### 1.创建项目并清空多余的东西

​	用vue UI 安装axios，element-ui，vuex，less插件

### 2.设置App.vue

在App.vue里面搭起框架，用element-ui的布局容器

```vue
<template>
  <div id="app">
    <el-container>
      <el-header class="header">
        <!-- header头部组件 -->
        <Header></Header>
      </el-header>
      <el-main>
        <!-- 路由出口 -->
       <router-view></router-view>
      </el-main>
    </el-container>
   
  </div>
</template>

<script>
// 引入header组件
import Header from '@/components/header'
export default {
  name: 'app',
  components: {
    Header
  }
}
</script>

<style>
html,
body,
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* 设置最小宽度 */
  min-width: 1100px;
}
.el-main{
  /* 设置main的背景颜色 */
  background-color: #f4f4f4;
}
</style>

```

3.配置Home页面组件并在router/index.js里面注册

Home.vue

```

```



主要思路

将登录请求放在vuex中的actions，请求成功后，将用户id放在state中，然后在其函数内部继续用得到的用户id发起用户信息请求，请求的结果继续放在state中，header与classify组件都请求state中的用户信息，mine路由页面也请求，更新用户信息请求不放在vuex中，而是直接将axios挂载到vue中，直接发起请求，更改成功后，再次引入actions中的请求用户信息组件，完成state里面的用户信息更新，从而响应式到各个组件的用户信息都更新



VueX的使用

