# canvas

## Element

`<canvas id="canvas" width height></canvas>`

## 渲染

```javascript
var canvas = document.querySelcet('#canvas')
var ctx = canvas.getContext('2d')
```

## 绘制

### 绘制矩形

```javascript
fillRect(x, y, width, height)
绘制一个填充的矩形
strokeRect(x, y, width, height)
绘制一个矩形的边框
clearRect(x, y, width, height)
清除指定矩形区域，让清除部分完全透明。
```

### 绘制线

`lineTo(x,y)`

### 绘制圆弧

```javascript
arc(x, y, radius, startAngle, endAngle, anticlockwise)
画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
arcTo(x1, y1, x2, y2, radius)
根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。


这里详细介绍一下arc方法，该方法有六个参数：x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise 为一个布尔值。为true时，是逆时针方向，否则顺时针方向。

注意：arc()函数中的角度单位是弧度，不是度数。角度与弧度的js表达式:radians=(Math.PI/180)*degrees。
```



### 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。使用路径绘制图形需要一些额外的步骤。

1. 首先，你需要创建路径起始点。
2. 然后你使用[画图命令](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D#Paths)去画出路径。
3. 之后你把路径封闭。
4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

```javascript
beginPath()
新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
closePath()
闭合路径之后图形绘制命令又重新指向到上下文中。
stroke()
通过线条来绘制图形轮廓。
fill()
通过填充路径的内容区域生成实心的图形。
当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调stroke()时不会自动闭合。
```

## 样式和颜色

### 色彩

```javascript
fillStyle = color
设置图形的填充颜色。
strokeStyle = color
设置图形轮廓的颜色。
```

### 透明度

`globalAlpha = transparencyValue`

这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。

### 线型

```javascript
lineWidth = value
设置线条宽度。默认1.0

lineCap = type
设置线条末端样式。三个：butt,round,square

lineJoin = type
设定线条与线条间接合处的样式。round,bevel,miter

miterLimit = value
限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。
getLineDash()
返回一个包含当前虚线样式，长度为非负偶数的数组。
setLineDash(segments)
设置当前虚线样式。
lineDashOffset = value
设置虚线样式的起始偏移量。

```

