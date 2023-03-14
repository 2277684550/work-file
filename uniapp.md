##  pages.json页面配置

与微信小程序相似，这里可以配置每个页面的标题头、颜色、背景等

哪个文件在前面，先显示哪个

globalStyle为全局配置，如果单个页面不配置的话，则默认为全局配置样式

```json
{
	"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		{
			"path": "pages/index/index",
			"style": {
				"navigationBarTitleText": "uni-app基础入门"
			}
		},
		{
		    "path" : "pages/list/list",
		    "style" :                                                                                    
		    {
		        
		        "navigationBarBackgroundColor":"#e37065"
		    }
		    
		}
	    
    ],
	"globalStyle": {
		"navigationBarTextStyle": "green",
		"navigationBarTitleText": "uni系列课程",
		"navigationBarBackgroundColor": "#fffae8",
		"backgroundColor": "#F8F8F8"
	},
	"uniIdRouter": {}
}

```

hbuilderx运行小程序得在manifest.json中配置小程序的id

![image-20230222083442944](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230222083442944.png)

scroll-view盒子

使元素能够超出屏幕，在手机端能够实现从左朝右的滑动效果

```html
<!--scroll-x scroll-y  横向纵向都能滑动 -->
		<scroll-view scroll-x scroll-y  class="scroll">
			<view class="group">
				<view class="item">1</view>
				<view class="item">2</view>
				<view class="item">3</view>
				<view class="item">4</view>
			</view>
		</scroll-view>
```

```scss
.scroll{
		border: 1px red solid;
		box-sizing: border-box;
		width: 750rpx;
		.group{		
			display: flex;
			.item{
				width: 300rpx;
				height: 200rpx;
				background-color: skyblue;
				margin-right: 10rpx;
				flex-shrink: 0;
			}
		}
	}
```

![image-20230222084012339](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230222084012339.png)

## swiper轮播与image

<swiper>

​	<swiper-item>

​	</swiper-item>

</swiper>

与小程序上面的相似

具体内容查看文档 https://uniapp.dcloud.net.cn/component/swiper.html

image与img不同，image有默认的宽高，如果你不设置的话，它是默认的

image是一个盒子，可以按照自己的想法显示里面的图片展示方法，缩放或是剪切

文档：https://uniapp.dcloud.net.cn/component/image.html

重要的是它的，mode属性

示例：

```vue
<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="swiper">
			<swiper-item>
				<image src="//m15.360buyimg.com/mobilecms/s1062x420_jfs/t1/134131/11/35396/58475/63f47a25F08e58699/322658ad9611a03a.jpg!cr_1053x420_4_0!q70.jpg" mode="aspectFit"></image>
			</swiper-item>
			<swiper-item>
				<image src="//m15.360buyimg.com/mobilecms/s1062x420_jfs/t1/131926/15/25947/50682/630882a7E3b00d35c/7093c8692f6386d3.jpg!cr_1053x420_4_0!q70.jpg" mode="aspectFit"></image>
			</swiper-item>
			<swiper-item>
				<image src="//m15.360buyimg.com/mobilecms/jfs/t1/5760/10/18446/121278/63e4f401F037b6ba9/515e63a450442fb7.jpg!cr_1125x449_0_166!q70.jpg" mode="aspectFit"></image>
			</swiper-item>
		</swiper>
```

```css
.swiper{
		height: 300rpx;
		width: 100%;
		padding: 0 20rpx;
		box-sizing: border-box;
		swiper-item{			
			image{
				height: 100%;
				width: 100%;
			}
		}
	}
```

## Fom表单

里面有一些基础的button和input之类的

```html
	<button size="mini" type="primary" plain>提交</button>
		<button size="mini" type="default" loading>提交</button>
		<button size="mini" type="warn">提交</button>
		<input type="text">
```

primary是提交，绿色或者蓝色（平台不同），但是加上plain之后，就镂空了，只有边框

default是灰色的按钮，加上loading就是加载中，类似表单的提交过程

![image-20230222135840411](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230222135840411.png)

input框都是一些基础的，不再展示，这里建议都用uView组件库里面的组件，更全

uView组件库：https://www.uviewui.com/components/intro.html

## navigator页面跳转组件

三个页面之间的来回跳转

里面有个重要的open-type属性

navigate与redirect跳转不了已经配置了tabBar的页面

| 属性      | 属性                       | 作用                                                         |
| --------- | -------------------------- | ------------------------------------------------------------ |
| navigate  | 对应 uni.navigateTo 的功能 | 保留当前页面，跳转到应用内的某个页面，需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数，跳转后，左上角还可以显示返回按钮 |
| redirect  | 对应 uni.redirectTo 的功能 | 关闭当前页面，跳转到应用内的某个页面，路径后可以带参数，不再显示返回按钮 |
| switchTab | 对应 uni.switchTab 的功能  | 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面，路径后可以带参数，（**常用**） |
| reLaunch  | 对应 uni.reLaunch 的功能   | 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面，路径后不能带参数 |

index/index.vue

```vue
		<navigator url="../list/list">
			<button type="primary">跳转新闻列表</button>
			</navigator>
		<navigator url="../about/about" open-type="redirect">
			<button type="primary">跳转关于我们</button>
			</navigator>
```

list/list.vue

```vue
<navigator url="../index/index">跳转首页</navigator>
```

about/about.vue

```vue
		<navigator url="../index/index">
			<button type="primary">返回首页</button>
		</navigator>
		<navigator url="../list/list">
			<button type="primary">返回列表</button>
		</navigator>
```

## tabBar底部跳转

在pages.json中创建tabBar对象

color:字体颜色

selectedColor：跳转后字体颜色

list：数组形式，里面为要跳转的页面

​			每一个要跳转的页面都为一个对象

text：底部按钮名称

pagePath：需要跳转的地址

iconPath：底部按钮图标

selectedIconPath：跳转后底部按钮图标

```json
"tabBar": {
		"color": "#333",
		"selectedColor": "#015FF9",
		"list": [
			{
				"text": "首页",
				"pagePath": "pages/index/index",
				"iconPath": "static/images/1.png",
				"selectedIconPath": "static/images/4.png"
			},
			{
				"text": "列表",
				"pagePath": "pages/list/list",
				"iconPath": "static/images/2.png",
				"selectedIconPath": "static/images/5.png"
			},
			{
				"text": "我的",
				"pagePath": "pages/about/about",
				"iconPath": "static/images/3.png",
				"selectedIconPath": "static/images/6.png"
			}
		]
	},
```

## onload传参与vue的route路由传参区别

uni是通过navigator来进行跳转页面的，里面的url属性是跳转的地址

**传参**

地址后面可以添加需要传递的参数（路由传参效果）

```vue
<navigator url="../demo1/demo1?wd=uniapp">
    <button type="primary">跳转demo1</button>
</navigator>
```

也可以通过事件uni.navigateTo

```js

goDemo1(){
    uni.navigateTo({
  	 url:'/pages/demo1/demo1?wd=uniapp&author=多多'
    })
}

```

url里面可以=在跳转的路径后面直接？添加参数，如果想传两个参数就得加&符号

**接收参数**

如果单纯的h5页面可以在mounted里面用this.$route.query接受

但是小程序不支持这个，如果想让小程序接受参数就得用onLoad钩子

```js
//h5接收参数
// mounted(){
// 	console.log(this.$route.query);
// }
//小程序和h5都可以接受参数
    onLoad(e){
    console.log(e);
    }
```

但是如果想让onLoad无法向$route一样接受页面栈，

可以在onLoad里面用getCurrentPages()来接受页面栈

## uni.showToast提示框

更多详情请看官方文档：https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showToast.html

![image-20230305185425808](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305185425808.png)

```js
clickImg(){
				uni.showToast({
					// 弹框提示文字
					title:"感谢使用uniApp",
					// 提示框显示图标
					icon:'error',
					// 如果选择了icon,就可以自定义图标
					image:'/static/logo.png',
					// 弹出提示框时,不能操作其他东西
					mask:true,
					// 提示框持续时间
					duration:1500,
					// 成功回调函数
					success() {
						// 延时1.5秒触发
						setTimeout(()=>{
							uni.navigateTo({
								url:'/pages/about/about'
							})
						},(1500))
					}
				})
			}
```

![image-20230305185449739](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305185449739.png)

## uni.showLoading加载框

官方文档：https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showLoading.html

```js
onLoad() {
			// 使用这个函数,本身是不会停止的,除非手动停止
			uni.showLoading({
				title:'页面加载中',
				mask:true
			})
			// 2秒后,结束uni.showLoadin运行
			setTimeout(()=>{
				// 该函数停止uni.showLoading函数运行
				uni.hideLoading()
			},2000)
		},
```

![image-20230305205700771](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305205700771.png)

## uni.showModal选择按钮弹窗

官方文档：https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showModal.html

```js
clickImg(){	
				// 点击弹出选择框
				uni.showModal({
					title:'手机验证',
					content:'我是主内容',
					// 按钮点击后成功触发,用success
					success(res) {
						console.log(res)
						// confirm为true则说明点的确定按钮
						if(res.confirm){
							// 点击确定跳转页面
							uni.navigateTo({
								url:'/pages/about/about'
							})
						}else{//confirm为false,则说明点击的取消按钮
						// 点击取消,弹出取消提示框
							uni.showToast({
								title:'取消',
								icon:'none'
							})
						}
					}
				})
			}
```

![image-20230305212742480](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305212742480.png)

```js
clickImg(){	
				// 带输入框的选择按钮
				uni.showModal({
					title:'手机验证码',
					// 是否显示输入框
					editable:true,
					// 输入框提示内容
					placeholderText:'请输入验证码',
					success(res) {
						// 打印输入的内容
						console.log(res.content);
					}
				})
			}
```

![image-20230305212639548](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305212639548.png)

## uni.showActionSheet底部弹出列表框

文档：https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showActionSheet.html

需要data里面定义数组

```js
clickImg(){
				uni.showActionSheet({
					itemList:this.arr,
					success:(res)=>{
						// res.tapIndex返回的是点击的下标,this.arr[]里面加入下标,就可以打印点击的内容
						console.log(this.arr[res.tapIndex]);
					}
				})
}				
```

![image-20230305214740580](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305214740580.png)

## 导航栏设置

![image-20230305221405365](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305221405365.png)

```js
onLoad() {
			// 设置导航栏文字
			uni.setNavigationBarTitle({
				title: '关于我们'
			});
			uni.setNavigationBarColor({
				// 导航栏文字颜色
				frontColor:'#ffffff',
				// 背景颜色
				backgroundColor:'#000000',
				// 背景颜色渐变动画
				animation:{
					duration:400,
					// 由低到快
					timingFunc:'easeIn'
				}
			})
			// 导航栏加载动画
			uni.showNavigationBarLoading()
			// 取消返回按钮
			// uni.hideHomeButton()
		}
```



## tab右上角显示文字

![image-20230305232119522](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305232119522.png)

在App.vue里面设置

因为我们想首次进入页面时，tab就显示这些东西，所有需要在app里面设置

```js
	// 页面加载第一次时执行
		onLaunch: function() {
			// 右上角显示文字
			uni.setTabBarBadge({
				// 从左开始算,第几个tar元素
			  index: 2,
			  // 设置的文本
			  text: '1'
			})
			// 右上角显示红点
			uni.showTabBarRedDot({
				index:1
			})
		},
```



然后我们想在进入标有红点或者红字的页面后，红点消失

![image-20230305232403924](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305232403924.png)

第二个tab是about.vue

我们进入about.vue页面后，在onload里面取消该红点

```css
onLoad() {
			//移除红点
			uni.hideTabBarRedDot({
				index:1
			})
		}
```

第三个是mine.vue,方式与上面相同，只是方法不一样

![image-20230305232438260](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230305232438260.png)

在mine.vue里面

```css
onLoad() {
    		//移除红字
			uni.removeTabBarBadge({
				index:2,
				
			})
		}
```

## uni.request网络请求

```js
	onLoad() {
			// 与request搭配使用,请求之前显示加载中
			uni.showLoading({
				title:'页面加载中'
			})
			uni.request({
				url:`https://api.thecatapi.com/v1/images/search`,
				// 请求方式
				method:'get',
				// 请求参数
				data:{
					limit:3
				},
				// 接口调用成功
				success:(res)=> {
					this.img=res.data
				},
				// 接口调用失败
				fail:(err)=>{
					
				},
				// 接口调用无论成功或者失败都执行
				complete:(data)=>{
					// 请求之后显示页面,取消加载中框
					uni.hideLoading()
				}
			})
		},
```

## 数据缓存到本地

常用**同步储存**：uni.setStorageSync()

```js
uni.setStorageSync('id','1')
```

常用**同步读取：**uni.getStorageSync('id')

```js
let id = uni.getStorageSync('id')
```

**删除指定缓存**：uni.removeStorage()

```js
uni.removeStorage({
				key:'id'
			})
```

**删除所有缓存**：uni.clearStorageSync()

```js
uni.clearStorageSync()
```

