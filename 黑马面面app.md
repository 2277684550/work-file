下载css和js文件

 https://gitee.com/xiaoqiang001/heimammsu.git

## 准备工作

#### 1.1  技术方案

```css
1. 弹性盒子 + rem + LESS 
4. 最小适配设备为iphone5 320px  最大设配设备为iphone8plus(ipad能正常查看内容即可)
```

#### 1.2 代码规范

```css
1. 类名语义化,尽量精短、明确，必须以字母开头命名，且全部字母为小写，单词之间统一使用下划线“_” 连接
2. 类名嵌套层次尽量不超过三层
3. 尽量避免直接使用元素选择器
4. 属性书写顺序
   布局定位属性：display / position / float / clear / visibility / overflow
   尺寸属性：width / height / margin / padding / border / background
   文本属性：color / font / text-decoration / text-align / vertical-align
   其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow
5. 避免使用id选择器
6. 避免使用通配符*和!important
```

#### 1.2 目录规范

```css
项目文件夹：heimamm
	样式文件夹：css
	业务类图片文件夹：images
	样式类图片文件夹： icons
	字体类文件夹： fonts
	js文件夹：js
	项目入口文件：index.html
```

## 部署

将css文件和js文件从素材里面拉出来放在css和js文件夹里

并创建css文件index.less

这里需要下载插件

![image-20230216163005469](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230216163005469.png)

下载好以后，ctrl+s保存index.less文件，本地就会创建一个同名的css文件

![image-20230216162903618](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230216162903618.png)

然后写入html和css代码

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>黑马面面</title>
    <!-- normalize.css在浏览器不同时，提高兼容性，减少bug，优化css -->
    <link rel="stylesheet" href="./css/normalize.css">
    <!-- 这里引入的是css文件，而不是less文件 -->
    <link rel="stylesheet" href="./css/index.css">
    <link rel="stylesheet" href="./css//swiper.min.css">
</head>
<body>
      <div></div>
</body>
<!-- 适配移动端的js框架，根据制不同的width给网页中html根节点设置不同的font-size，
然后所有的px都用rem来代替，这样就实现了不同大小的屏幕都适应相同的样式了 -->
<script src="./js/flexible.js"></script>
<script src="./js/swiper.min.js"></script>
</html>
```

index.less

这里也需要下载一个插件

![image-20230216164837166](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230216164837166.png)

将其基准设置为媒体查询所设置的基准

![image-20230216164923053](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230216164923053.png)

此时1rem便等于37.5px，把鼠标放在所需要更改的px行上 Alt+Z,便可以将px转为rem，再次 Alt+Z 便可以转换回来

媒体查询基准不能转换为rem，因为插件的rem是以它为基准，改为rem后，插件的基准就不生效了

```less
body{
    // 设置屏幕的最小最大宽度，并居中
    min-width: 320px;
    max-width: 750px;
    margin: 0 auto;
    height: 1200px;
}
// 媒体查询
// 约束当屏幕大于750px 的时候，html的字体大小就不变化了
@media screen and (min-width:750px){
    html{
        // 当屏幕最小为750px时，html的文字只能为37.5px
        // 也就是说超过750px之后，文字也不会放大了
        // !important提高指定样式规则的应用优先权（优先级）
        font-size: 37.5px !important ;
    }
}
div{
    width: 2.6667rem;
    height:2.6667rem;
    background-color: aquamarine;
}
```

为什么是字体大小为37.5px呢？

​    因为flexible.js将屏幕的宽度分成了十份

​    我们这里是以iphone为例，iphone6/7/8的宽度为375

​    所以每一份就是37.5px，我们以每一份的大小为字体的大小

​	如果是以其他的手机为例，宽度是750px，那么每一份设置成75px，

​	字体大小设置成75px也没问题，因为只是一个字体与屏幕大小的bi'li