![image-20230223161443360](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223161443360.png)

如果想让容器自适应屏幕，便可以在class里面多绑定一个，如class=“col-md-4  col-sm-4  col-lg-4”让容器在小屏幕、中屏幕、大屏幕里面都是同样的分割

![image-20230223161308348](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223161308348.png)

![image-20230223161350884](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223161350884.png)

![image-20230223161329300](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223161329300.png)

![image-20230223162632977](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223162632977.png)

![image-20230223164054431](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223164054431.png)

![image-20230223164309737](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223164309737.png)

![image-20230223164728784](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223164728784.png)

![image-20230223165751468](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223165751468.png)

![image-20230223174653142](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223174653142.png)

![image-20230223174710353](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223174710353.png)

```html
<!-- table(行边框) table-bordered(列边框) table-striped(斑马纹) table-hover(高亮) table-condensed(紧凑) -->
		<table class="table table-bordered table-striped table-hover table-condensed">
			<tr>
				<td>面向对象</td>
				<td>面向你</td>
				<td>面向我</td>
			</tr>
			<tr>
				<td>面向对象</td>
				<td>面向你</td>
				<td>面向我</td>
			</tr>
			<tr>
				<td>面向对象</td>
				<td>面向你</td>
				<td>面向我</td>
			</tr>
		</table>
```

![image-20230223175549517](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223175549517.png)



![image-20230223183156942](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223183156942.png)

​											垂直方向必须再多加一个div盒子，不然不生效，水平方向只需要一个label即可

![image-20230223192026274](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223192026274.png)

​												单选框与复选框用法相同，只需要将checkbox改为radio即可

![image-20230223230404634](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223230404634.png)

​												禁用按钮加disabled属性

```html
				<button class="btn btn-primary btn-lg">大按钮</button>a
				<button class="btn btn-info btn-sm">中按钮</button>
				<button class="btn btn-success btn-xs">小按钮</button>3
					<!-- 禁用按钮 -->
				<button class="btn btn-primary" onclick="alert('666')" disabled="disabled">按钮</button>
```

![image-20230223230740261](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230223230740261.png)

#### 4.2.2.表单布局

基本的表单结构是 Bootstrap 自带的，个别的表单控件自动接收一些全局样式。下面列出了创建基本表单的步骤:
·向父<form>元素添加role="form”。
·把标签和控件放在一个带有 classform-group 的<div>中这是获取最佳间距所必需的。
·向所有的文本元素<input>、<textarea>和<select>添加cass ="form-contro"

#### 4.2.2.1.水平表单

同一行显示form-horizontal
配合Bootstrap框架的网格系统

<form class_"form-horizonta1" role"form">
<div class"form-group"><labe1 for="emai1”class="contro1-1abe] co1-sm-2">邮箱</1abe1><div class="co1-sm-10">
<input type="emai1" class="form-contryl”placeholder="请输入邮箱"/></div>
</div>
<div class="form-group"><label for="pwd”class="contro1-labe] co1-sm-2">密码</1abel><div class="col-sm-10"><input type="pwd”class="form-control”placeholder="请输入密码”/></div>
</div>
<div cTass="form-group">

