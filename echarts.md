# echarts

## 1.基本使用

![image-20230311181125626](md%E5%9B%BE%E7%89%87%E5%AD%98%E6%94%BE/image-20230311181125626.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="css/index.css">
</head>
<body>
    <!--2.设置一个容器 -->
    <div class="box"></div>
</body>
<!-- 1.引入echarts.js文件 -->
<script src="./js/echarts.js"></script>
<script>
    // 3.实例化echarts对象
    var myChart = echarts.init(document.querySelector('.box'));
     // 4.指定图表的配置项和数据
     var option = {
        title: {
          text: 'ECharts 入门示例'
        },
        tooltip: {},
        legend: {
          data: ['销量']
        },
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [
          {
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      };

      // 5.将配置项和数据设置给echarts。
      myChart.setOption(option);
</script>
</html>
```

![image-20230311174222593](C:/Users/22776/AppData/Roaming/Typora/typora-user-images/image-20230311174222593.png)

## 2.配置一个图表

```js
option = {
  //设置线条的颜色，后面是个数组
  color:['pink','red','green','skyblue','black']
  //设置图标的标题
  title: {
    text: 'Stacked Line'
  },
  //图标的提示框组件
  tooltip: {
    trigger: 'axis'
  },
  //最上层的图例组件
  legend: {
    //如果series里面name值，那么这里的data可以不要
    // data: ['Email', 'Union Ads', 'Video Ads', 'Direct', 'Search Engine']
  },
  //网格配置  通过控制与盒子的距离来控制图标的大小
  grid: {
    left: '3%',
    right: '4%',
    bottom: '3%',
    containLabel: true
  },
  //工具箱组件，可以另存为图片
  toolbox: {
    feature: {
      saveAsImage: {}
    }
  },
  //设置x轴的相关配置
  xAxis: {
    //显示文字，而不是数字
    type: 'category',
    // 是否与左侧容器有间隔
    boundaryGap: false,
    data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  },
  //设置y轴的相关配置
  yAxis: {
    type: 'value'
  },
  
  series: [
    {
      name: 'Email',
      // 类型
      type: 'line',
      //是否显示总量，也就是下面的每一条数据都与这里的累加，所以看起来不会重叠，一般不需要
      // stack: 'Total',
      //每条折线的数据
      data: [120, 132, 101, 134, 90, 230, 210]
    },
    {
      name: 'Union Ads',
      type: 'line',
      stack: 'Total',
      data: [220, 182, 191, 234, 290, 330, 310]
    },
    {
      name: 'Video Ads',
      type: 'line',
      stack: 'Total',
      data: [150, 232, 201, 154, 190, 330, 410]
    },
    {
      name: 'Direct',
      type: 'line',
      stack: 'Total',
      data: [320, 332, 301, 334, 390, 330, 320]
    },
    {
      name: 'Search Engine',
      type: 'line',
      stack: 'Total',
      data: [820, 932, 901, 934, 1290, 1330, 1320]
    }
  ]
};
```

