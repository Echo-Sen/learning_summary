# 可视化ECharts

## 基础

### day1



Echatrs 的快速上手：

1.引入 echarts.js 的文件

2.准备呈现图表的盒子

`<div style="width: 600px;height:400px;">></div>`

3.初始化 echarts 实例对象

`let mChart = echarts.init(doucument.queryselector('div'))`

4.准备配置项

`var option = {`

xAxis: {

type: 'category',

data: ['小明', '小红', '小王']

},

`yAxis: {
	type: 'value'
	},
	series: [
	{
	name: '语文',
	type: 'bar',
	data: [70, 92, 87],
	}
	]`

`}`

5.将配置项设置给 echarts 对象

`mChart.setOption(option)`





#### 柱状图



##### 常见效果

标记：

最值：markPoint   .type:'max'      .name:'最大值'

平均值：markLine  .type:'average'   .name:'平均值'

数值显示：label .show是否可见  .rotate旋转角度

柱宽度：barwidth  '30%'



标题：title 



提示框：tooltip

​	触发类型：trigger

​		可选值有item\axis

​	触发时机: triggerOn

​		可选值有 mouseOver\click

​	格式化显示: formatter	

​		字符串模板

​		回调函数



工具按钮: toolbox

​	toolbox 是 ECharts 提供的工具栏, 内置有 导出图片，数据视图, 重置, 数据区域缩放, 动态类型切

​	换五个工具

​	工具栏的按钮是配置在 feature 的节点之下

```javascript
toolbox: {

​	feature: {

​	saveAsImage: {}, // 将图表保存为图片

​	dataView: {}, // 是否显示出原始数据

​	restore: {}, // 还原图表

​	dataZoom: {}, // 数据缩放

​	magicType: { // 将图表在不同类型之间切换,图表的转换需要数据的支持

​	type: ['bar', 'line']

​	}

​	}

​}
```

​	

图例: legend

​	legend 中的 data 是一个数组

​	legend 中的 data 的值需要和 series 数组中某组数据的 name 值一致





#### 折线图

标注区间 markArea

平滑线条 smooth

线条样式 lineStyle

填充风格 areaStyle

紧挨边缘 boundaryGap

摆脱0值比例 scale 

堆叠图 stack



#### 散点图

处理数据

```javascript
var axisData = []
for (var i = 0; i < data.length; i++) {
var height = data[i].height
var weight = data[i].weight
var itemArr = [height, weight]
axisData.push(itemArr)
}
```

axisData 就是一个二维数组, 数组中的每一个元素还是一个数组, 最内层数组中有两个元素



 调整配置项, 脱离0值比例



气泡图效果

symbolSize 控制散点的大小

itemStyle.color 控制散点的颜色



涟漪动画效果

type:effectScatter

​	将 type 的值从 scatter 设置为 effectScatter 就能够产生涟漪动画的效果

rippleEffect

​	rippleEffect 可以配置涟漪动画的大小

showEffectOn

​	showEffectOn 可以控制涟漪动画在什么时候产生, 它的可选值有两个: render 和 emphasis

​	render 代表界面渲染完成就开始涟漪动画

​	emphasis 代表鼠标移过某个散点的时候, 该散点开始涟漪动画



#### 直角坐标系

网格 grid

坐标轴 axis

区域缩放 dataZoom

​	区域缩放类型 type

​		slider : 滑块

​		inside : 内置, 依靠鼠标滚轮或者双指缩放

​	产生作用的轴

​		xAxisIndex :设置缩放组件控制的是哪个 x 轴, 一般写0即可

​		yAxisIndex :设置缩放组件控制的是哪个 y 轴, 一般写0即可

​	指明初始状态的缩放情况

​		start : 数据窗口范围的起始百分比

​		end : 数据窗口范围的结束百分比



#### 饼图

显示数值

​	label.show : 显示文字

​	label.formatter : 格式化文字

南丁格尔图

​	南丁格尔图指的是每一个扇形的半径随着数据的大小而不同, 数值占比越大, 扇形的半径也就越大

​	roseType:'radius'

选中效果

​	selectedMode: 'multiple'

​		选中模式，表示是否支持多个选中，默认关闭，支持布尔值和字符串，字符串取值可

​		选 'single' ， 'multiple' ，分别表示单选还是多选

​	selectedOffset: 30

​		选中扇区的偏移距离

圆环

​	radius

​	饼图的半径。可以为如下类型：

​		number ：直接指定外半径值。 string ：例如， '20%' ，表示外半径为可视区尺寸（容器高宽中

​		较小一项）的 20% 长度。 Array. ：数组的第一项是内半径，第二项是外半径, 通过 Array , 可以

​		将饼图设置为圆环图

​		

#### 地图

百度地图API : 使用百度地图的 api , 它能够在线联网展示地图, 百度地图需要申请 ak

矢量地图 : 可以离线展示地图, 需要开发者准备矢量地图数据



1. ECharts 最基本的代码结构

2. 准备中国的矢量 json 文件, 放到 json/map/ 目录之下

3. 使用 Ajax 获取 china.json

4. 在Ajax的回调函数中, 往 echarts 全局对象注册地图的 json 数据

   echarts.registerMap('chinaMap', chinaJson)

5.  获取完数据之后, 需要配置 geo 节点, 再次的 setOption

   type : 'map'

   map : 'chinaMap'



缩放拖动: roam

名称显示: label

初始缩放比例: zoom

地图中心点: center



地图根据数据显示颜色：

1.显示基本的中国地图

2.准备好城市空气质量的数据, 并且将数据设置给 series

3.将 series 下的数据和 geo 关联起来

  geoIndex: 0

  type: 'map'

4.结合 visualMap 配合使用

  visualMap 是视觉映射组件, 和之前区域缩放 dataZoom 很类似, 可以做数据的过滤. 只不过

  dataZoom 主要使用在直角坐标系的图表, 而 visualMap 主要使用在地图或者饼图中



地图和散点图结合

1.给 series 这个数组下增加新的对象

2.准备好散点数据,设置给新对象的 data

3.配置新对象的 type

​	type:effectScatter

​	让散点图使用地图坐标系统

​		coordinateSystem: 'geo'

​	让涟漪的效果更加明显

​		rippleEffect: { scale: 10 }