# 1.axios封装和请求响应劫持

## 1.1 创建全局配置js文件

![image-20221217174708140](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217174708140.png)

```js
export default {
    title:'lcelm', //项目的名称
    baseUrl:{
        dev:'http://localhost:3000', //开发环境后台接口地址
        pro:"" //生产环境，上线产品发布之后，后台接口地址
    }
}
```

## 1.2 创建要封装的js文件

![image-20221217173205080](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217173205080.png)

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

## 1.3 创建所有axios请求js文件

![image-20221217183216745](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217183216745.png)

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

## 1.4 调用axios请求

![image-20221217183942739](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217183942739.png)

```vue
<template>
  <div class="home">
   <h1>HOME</h1>
  </div>
</template>

<script>
// 引入axios请求文件
import { getBannerData } from '@/api/data';
export default {
  name: 'HomeView',
  async mounted(){
    // 拿到请求完成的getBannerData数据
    let result = await getBannerData()
    console.log(result);
  }
}
</script>

```

这里报错是因为我们没有配置3000端口的后台

![image-20221217184433188](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217184433188.png)

## 1.5 创建后端服务

#### 1. 在项目的同级目录下创建文件夹并创建index.js文件

![image-20221217185330497](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217185330497.png)

#### 2. 安装express  

`cnpm install express -- save`

![image-20221217184916432](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217184916432.png)

![image-20221217185025152](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217185025152.png)

#### 3. 允许跨域

如果不跨域，请求便会报错

![image-20221217190224164](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217190224164.png)

​	因为浏览器为了安全考虑，不允许8080端口请求3000服务器的端口，因为默认的，只能请求自己的，

​	但是我们想要让8080端口访问3000服务器端口，便需要跨域

​	`npm install cors`

![image-20221217185121544](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217185121544.png)

#### 4.创建服务

server/index.js

```js
// 1.引入express框架
let express = require('express')
// 2.用express实例化一个app对象
let app = express()

// 允许跨域
const cors = require('cors')
app.use(cors()) 

app.get('/',(req,res) => {
    // res为要返回的数据
    res.json({
        msg:'这是首页'
    })
})

app.get('/banner',(req,res) => {
    res.json({
        msg:'这是banner'
    })
})

// 监听刚才写的3000端口，并回调输出地址
app.listen(3000,() => {
    console.log('http://localhost:3000');
})
```

#### 5.启动服务器

`node 要启动的js文件服务器`

![image-20221217185836895](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217185836895.png)

![image-20221217190053032](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217190053032.png)

# 2.开发模式跨域解决方案与代理服务器

## 1.代理服务器

因为我们的项目是启动在8080服务器端口

![image-20221217220719850](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217220719850.png)

它要直接调用我们自建的3000服务器端口的数据就会产生跨域，

那么我们可以让8080服务器去请求3000服务器数据，而不是之间调用，

然后8080返回请求的数据转发给浏览器，便不会跨域

此时8080服务器便是代理服务器了，

## 2.开发模式跨域

### 1.在项目中创建一个vue.config.js文件

vue.config.js

```js
module.exports = {
  // devServer 开发服务器
  devServer:{
    proxy:{
      // 专门访问/api 地址的都用代理进行操作
      '/api':{
        // 代理的真正服务器
        target:'http://localhost:3000',
        // 因为我们真正的axios请求没有api这个地址，所以要对我们自己axios请求地址进行重写
        // 匹配到/api字符串然后把它替换成 :
        pathRewrite:{
          '^/api':""
        }
      }
    }
  }
}
```

devServer 为vue Cli 里面的配置属性 ，用来开发服务器  https://cli.vuejs.org/zh/config/#devserver-proxy

### 2. 改变我们的全局配置的js文件

![image-20221217224407838](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217224407838.png)

### 3. 总结：什么是服务器代理

![image-20221217224507995](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221217224507995.png)

我们发现最后banner发起的请求是http://localhost:8080/api/banner,最终访问的是8080服务器端口

因为我们在全局配置的js文件里面将我们的开发环境后台地址改成了/api/,于是我们的每一次请求都会是http://localhost:8080/api/,

此时我们的每一次请求都会把请求地址追加到http://localhost:8080/api/后面，变成了http://localhost:8080/api/banner

但是我们在步骤1的vue.config.js里面规定了只要是带有/api 的请求地址，全部都进行服务器代理，把真实请求的服务器指向 http://localhost:3000/api/

然后有对/api进行了重写，让/api字符串为空，最后的真实请求地址变为http://localhost:3000

此时我们的banner请求便为http://localhost:3000/banner  此时便可以请求到3000服务器端口的数据了，

然后我们的8080服务器转发这些数据到浏览器

相当于8080服务器是一个中间人，浏览器问它要数据，它说我没有，但是它可以去向3000服务器借，借到的数据再给浏览器，

这就是服务器代理



# 3.mockjs模拟后端数据实现前后端同时开发

mockjs是直接劫持某个地址请求，然后给它返回数据，该请求便去不了服务器拿数据了，而是只去mockjs拿数据

打开mock官网安装页面  https://github.com/nuysoft/Mock/wiki/Getting-Started

运行命令  `# npm install mockjs`

![image-20221218095310328](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218095310328.png)

## 1.创建mock的js文件

![image-20221218100857194](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218100857194.png)

```js
// 引入mockjs
import Mock from 'mockjs'

// 配置请求延时，因为请求是有延迟的，不能直接就把Mock给显示出来，不然会出一些问题，应该让Mock等待一下
Mock.setup({
    timeout:1000
})

//直接使用字符串匹配路径还可以使用正则表达匹配路径 
// Mock.mock('/api/user',{
//     username:'韩',
//     age:18,
//     gender:'男',
//     type:'帅'
// })
// igs代表全局
// Mock.mock(/\/api\/user/igs,{
//     username:'韩',
//     age:18,
//     gender:'男',
//     type:'帅'
// })

Mock.mock(/\/api\/banner/igs,{
data:{
    "mtime":"@datetime",//生成随机的时间
    "score |1-800":800,//生成随机的成绩 1-800分
    "rank |1-100":100,//生成随机的等级 1-100
    "nickname":"@cname",//生成随机的名字
    "address":"@url",//随机的地址
    "imgUrl":"@image",//随机的图片地址
    "email":"@email"//随机的邮箱
    },
    meta:{
        status:200
    }
})

```

## 2.在main.js 里面判断什么时候使用mockjs

```js
import Vue from 'vue'
import './plugins/axios'
import App from './App.vue'
import router from './router'
import './plugins/element.js'


Vue.config.productionTip = false
// 如果是开发环境，就引入mock
if(process.env.NODE_ENV === 'development') require('@/api/mock')

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')

```

## 3.调用数据

![image-20221218101457959](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218101457959.png)

![image-20221218101421635](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218101421635.png)

# 4.REM布局

通过改变字体的大小来使页面适应不同的页面大小

REM布局就是通过改变根元素的大小来使所有元素的大小都改变

## 1.创建rem.js 文件

![image-20221218110322798](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218110322798.png)

```js
// 定义一个匿名函数，并让它自执行
(function(){
    function resize(){
        var baseFontSize = 100; //设计稿100px相对于1rem，那么750px相当于7.5rem-->相当于各种屏幕的100%的宽度
        var designWidth = 750; //Iphone手机的宽度为750px，实际显示为375px是因为Iphone为了字体更大，把2px合成了1px，所以最终显示375px
        var width = window.innerWidth; //屏幕的宽度
        var currentFontSize = (width/designWidth)*baseFontSize;
        // 更改显示页面的根元素字体大小
        document.querySelector('html').style.fontSize = currentFontSize + 'px'
    }

    // 当屏幕调整的时候修改根元素字体的大小
    window.onresize = function(){
        resize()
    }

    // 当页面文档加载进来的时候也要修改根元素字体大小，使所有的元素都改变大小
    document.addEventListener('DOMContentLoaded',resize)
}())
```

**并在main.js 引入rem.js文件**

![image-20221218110428784](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218110428784.png)

## 2.修改页面入口App.vue body标签 的字体大小

为标准 16px

![image-20221218110626283](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218110626283.png)

## 3.页面开发全部使用rem布局

![image-20221218110727493](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218110727493.png)

![image-20221218110738799](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218110738799.png)

![image-20221218110747093](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218110747093.png)

此时页面无论大小如何

HOME所在的h1标签始终占比页面的50%

# 5.首页数据获取

在mock.js文件里面模拟数据，但是只模拟入口名称，里面的文件单独存放一个文件夹存放数据

## 1.在data.js里面增加请求

![image-20221218120054400](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218120054400.png)

```js

export const getPosiData= () => {
    // 返回我们用axios去请求的内容，axios里面现在有我们封装好的request方法，可以实例化instance对象，
    // instance对象把我们自己定义的配置进行导入，然后覆盖掉原本的默认配置
    return axios.request({
        url:'posi',//请求地址
        methods:'get'//请求方法
    })
}

export const getgoodsData= () => {
    // 返回我们用axios去请求的内容，axios里面现在有我们封装好的request方法，可以实例化instance对象，
    // instance对象把我们自己定义的配置进行导入，然后覆盖掉原本的默认配置
    return axios.request({
        url:'goods',//请求地址
        methods:'get'//请求方法
    })
}
```

## 2.新建一个文件夹，里面存放每个请求的数据

![image-20221218120438612](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218120438612.png)



## 3.在mock.js里面模拟数据入口

引入要请求的数据

![image-20221218120236947](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218120236947.png)

模拟数据的入口，并把引入的数据传进来

![image-20221218120254158](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218120254158.png)

## 4.调用数据

HomeView.vue

![image-20221218120636646](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218120636646.png)



获取成功

![image-20221218120658092](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218120658092.png)

# 6.使用矢量图标

![image-20221218133452993](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133452993.png)

![image-20221218133513001](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133513001.png)

![image-20221218133526277](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133526277.png)

![image-20221218133550648](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133550648.png)

解压文件

![image-20221218133642889](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133642889.png)

打开demo_index.html文件可以查看教程和图标的信息

![image-20221218133851360](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133851360.png)

把要用的文件复制到public文件夹下的css文件下面，没有css文件夹就创建一个

![image-20221218133724008](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133724008.png)

在public的index.html 文件中引入css文件

![image-20221218133813180](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133813180.png)

在要用到的vue文件里面使用图标

![image-20221218133929829](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133929829.png)

使用成功

![image-20221218133942565](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20221218133942565.png)
