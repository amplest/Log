# canvas 笔记

## 绘制矩形

- `fillRect(x,y,width,height)` 绘制填充的矩形
- `strokeRect(x, y, width, height)` 绘制矩形的边框
- `clearRect(x, y, width, height)` 清除指定矩形区域，让清除部分完全透明

## 绘制路径

- `beginPath()` 新建一条路径，生成后，图形绘制命令被只想到路径上生成路径
- `closePath()` 闭合路径之后图形绘制命令又重新指向到上下文中
- `stroke()` 通过线条来绘制图形轮廓
- `fille()` 通过填充路径的内容区域生成实心的图形

*注意：当前路径为空，即调用beginPath()之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是moveTo（），无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。*

*注意：当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。*

## 移动笔触

- `moveTo(x,y)` 将笔触移动到指定的坐标x以及y上

*当canvas初始化或者beginPath()调用后，你通常会使用moveTo()函数设置起点。我们也能够使用moveTo()绘制一些不连续的路径。*

## 线

- `lineTo(x,y)`

## 圆弧

- `arc(x,y,radius, startAngle, endAngle, anticlockwise)`

*Π是半圆弧线*

*画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成*

*x,y为绘制圆弧所在圆上的圆心坐标。radius为半径。startAngle以及endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准。参数anticlockwise为一个布尔值。为true时，是逆时针方向，否则顺时针方向*

*arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式:
弧度=(Math.PI/180)\*角度*

- `arcTo(x1, y1, x2, y2, radius)`

*根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点*

## 二次贝塞尔曲线及三次贝塞尔曲线

二次贝塞尔曲线有一个开始点（蓝色）、一个结束点（蓝色）以及一个控制点（红色），而三次贝塞尔曲线有两个控制点

![二次/三次](../static/canvas/Canvas_curves.png)

- `quadraticCurveTo(cp1x, cp1y, x, y)` 绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点
- `bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)` 绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点

## 矩形

- `rect(x, y, width, height)` 绘制一个左上角坐标为（x,y），宽高为width以及height的矩形

*当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置回默认坐标*

## Path2D

- `Path2D.addPath(path [, transform])` 添加了一条路径到当前路径（可能添加了一个变换矩阵）

``` js
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');

    var rectangle = new Path2D();
    rectangle.rect(10, 10, 50, 50);

    var circle = new Path2D();
    circle.moveTo(125, 35);
    circle.arc(100, 35, 25, 0, 2 * Math.PI);

    ctx.stroke(rectangle);
    ctx.fill(circle);
  }
}
```

## 色彩

- `fillStyle = color` 设置图形的填充颜色
- `strokeStyle = color` 设置图形轮廓的颜色

## 透明度 

- `globalAlpha = transparencyValue` 有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0

## 线型

-  `lineWidth = value` 设置线条宽度
-  `lineCap = type` 设置线条末端样式 `butt , round , square 默认是 butt`
-  `lineJoin = type` 设定线条与线条间接合处的样式 `round , bevel , miter 默认是 miter`
-  `miterLimit = value` 限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度
-  `getLineDash()` 返回一个包含当前虚线样式，长度为非负偶数的数组
-  `setLineDash(segments)` 设置当前虚线样式
-  `lineDashOffset = value` 设置虚线样式的起始偏移量

## 渐变 

- `createLinearGradient(x1, y1, x2, y2)` 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)
- `createRadialGradient(x1, y1, r1, x2, y2, r2)` 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆
- `gradient.addColorStop(position, color)` addColorStop 方法接受 2 个参数，position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。color 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）

## 图案样式

- `createPattern(image, type)` 该方法接受两个参数。Image 可以是一个 Image 对象的引用，或者另一个 canvas 对象。Type 必须是下面的字符串值之一：repeat，repeat-x，repeat-y 和 no-repeat

## 阴影 

- `shadowOffsetX = float` shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
- `shadowOffsetY = float` shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0
- `shadowBlur = float` shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。
- `shadowColor = color` shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。

## Canvas 填充规则

当我们用到 fill（或者 clip和isPointinPath ）你可以选择一个填充规则，该填充规则根据某处在路径的外面或者里面来决定该处是否被填充，这对于自己与自己路径相交或者路径被嵌套的时候是有用的。

- `nonzero` non-zero winding rule, 默认值.
- `evenodd` even-odd winding rule.

``` js
fill('evenodd')
```

## 绘制文本

- `fillText(text, x, y [, maxWidth])` 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
- `strokeText(text, x, y [, maxWidth])` 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

## 有样式的文本

- `font = value` 当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 10px sans-serif
- `textAlign = value` 文本对齐选项. 可选的值包括：start, end, left, right or center. 默认值是 start
- `textBaseline = value` 基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。默认值是 alphabetic
- `direction = value` 文本方向。可能的值包括：ltr, rtl, inherit。默认值是 inherit

## 预测量文本宽度

- `measureText()` 将返回一个 TextMetrics对象的宽度、所在像素，这些体现文本特性的属性

## 获得需要绘制的图片

- `HTMLImageElement` 这些图片是由Image()函数构造出来的，或者任何的`<img>`元素
- `HTMLVideoElement` 用一个HTML的 `<video>`元素作为你的图片源，可以从视频中抓取当前帧作为一个图像
- `HTMLCanvasElement` 可以使用另一个 `<canvas>` 元素作为你的图片源。
- `ImageBitmap` 这是一个高性能的位图，可以低延迟地绘制，它可以从上述的所有源以及其它几种源中生成

### 使用相同页面内的图片

- `document.images`集合
- `document.getElementsByTagName()`方法
- 如果你知道你想使用的指定图片的ID，你可以用`document.getElementById()`获得这个图片

### 使用其它域名下的图片

- 在 `HTMLImageElement`上使用`crossOrigin`属性，你可以请求加载其它域名上的图片。
- 如果图片的服务器允许跨域访问这个图片，那么你可以使用这个图片而不污染canvas，否则，使用这个图片将会污染canvas。

### 使用其它 canvas 元素

- 和引用页面内的图片类似地，用 `document.getElementsByTagName` 或 `document.getElementById` 方法来获取其它 canvas 元素。但你引入的应该是已经准备好的 canvas。
- 一个常用的应用就是将第二个canvas作为另一个大的 canvas 的缩略图。

### 由零开始创建图像

``` js
var img = new Image();   // 创建img元素
img.onload = function(){
  // 执行drawImage语句
}
img.src = 'myImage.png'; // 设置图片源地址
```

## 绘制图片

- `drawImage(image, x, y)` 其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标

## 缩放 

- `drawImage(image, x, y, width, height)` 这个方法多了2个参数：width 和 height，这两个参数用来控制 当向canvas画入时应该缩放的大小

## 切片

- `drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)` 第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照下边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小

![Canvas_drawimage.jpg](./../static/canvas/Canvas_drawimage.jpg)

## 控制图像的缩放行为

Gecko 1.9.2 引入了 `mozImageSmoothingEnabled` 属性，值为 `false` 时，图像不会平滑地缩放。默认是 `true`

## 状态的保存和恢复

- `save()` 保存画布(canvas)的所有状态
- `restore()` save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照
- 包含如下属性
  - strokeStyle
  - fillStyle
  - globalAlpha
  - lineWidth
  - lineCap
  - lineJoin
  - miterLimit
  - lineDashOffset
  - shadowOffsetX
  - shadowOffsetY
  - shadowBlur
  - shadowColor
  - globalCompositeOperation
  - font
  - textAlign
  - textBaseline
  - direction
  - imageSmoothingEnabled

``` js
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  ctx.fillRect(0,0,150,150);   // 使用默认设置绘制一个矩形
  ctx.save();                  // 保存默认状态

  ctx.fillStyle = '#09F'       // 在原有配置基础上对颜色做改变
  ctx.fillRect(15,15,120,120); // 使用新的设置绘制一个矩形

  ctx.save();                  // 保存当前状态
  ctx.fillStyle = '#FFF'       // 再次改变颜色配置
  ctx.globalAlpha = 0.5;    
  ctx.fillRect(30,30,90,90);   // 使用新的配置绘制一个矩形

  ctx.restore();               // 重新加载之前的颜色状态
  ctx.fillRect(45,45,60,60);   // 使用上一次的配置绘制一个矩形

  ctx.restore();               // 加载默认颜色配置
  ctx.fillRect(60,60,30,30);   // 使用加载的配置绘制一个矩形
}
```

你可以调用任意多次 save方法。每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复

## 移动

- `translate(x, y)` 方法接受两个参数。x 是左右偏移量，y 是上下偏移量

## 旋转 

- `rotate(angle)` 这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值

*旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 `translate` 方法*

## 缩放

- `scale(x, y)` 方法可以缩放画布的水平和垂直的单位。两个参数都是实数，可以为负数，x 为水平缩放因子，y 为垂直缩放因子，如果比1小，会比缩放图形， 如果比1大会放大图形。默认值为1， 为实际大小

## 变形 

- `transform(a, b, c, d, e, f)`  这个方法是将当前的变形矩阵乘上一个基于自身参数的矩阵，如下面的矩阵所示
  - `a(m11)` 水平方向的缩放
  - `b(m12)` 水平方向的倾斜偏移
  - `c(m21)` 竖直方向的倾斜偏移
  - `d(m22)` 竖直方向的缩放
  - `e(dx)` 水平方向的移动
  - `f(dy)` 竖直方向的移动
- `setTransform(a, b, c, d, e, f)`  这个方法会将当前的变形矩阵重置为单位矩阵，然后用相同的参数调用 transform 方法。如果任意一个参数是无限大，那么变形矩阵也必须被标记为无限大，否则会抛出异常。从根本上来说，该方法是取消了当前变形,然后设置为指定的变形,一步完成
- `resetTransform()` 重置当前变形为单位矩阵，它和调用以下语句是一样的：`ctx.setTransform(1, 0, 0, 1, 0, 0);`

``` js
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  var sin = Math.sin(Math.PI/6); // 正弦
  var cos = Math.cos(Math.PI/6); // 余弦
  ctx.translate(100, 100);
  var c = 0;
  for (var i=0; i <= 12; i++) {
    c = Math.floor(255 / 12 * i);
    ctx.fillStyle = "rgb(" + c + "," + c + "," + c + ")";
    ctx.fillRect(0, 0, 100, 10);
    ctx.transform(cos, sin, -sin, cos, 0, 0);
  }
  
  ctx.setTransform(-1, 0, 0, 1, 100, 100);
  ctx.fillStyle = "rgba(255, 128, 255, 0.5)";
  ctx.fillRect(0, 50, 100, 100);
}
```

## [globalCompositeOperation](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Compositing)

- `globalCompositeOperation = type` 这个属性设定了在画新图形时采用的遮盖策略，其值是一个标识12种遮盖方式的字符串
  
## 裁切路径

- `clip()` 将当前正在构建的路径转换为当前的裁剪路径

## [基本的动画](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Basic_animations)

- 清空 canvas
  - 除非接下来要画的内容会完全充满 canvas （例如背景图），否则你需要清空所有。最简单的做法就是用 clearRect 方法。
- 保存 canvas 状态
  - 如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
- 绘制动画图形（animated shapes）
  - 这一步才是重绘动画帧。
- 恢复 canvas 状态
  - 如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧

## 有安排的更新画布

首先，可以用`window.setInterval()`, `window.setTimeout()`,和`window.requestAnimationFrame()`来设定定期执行一个指定函数。

- `setInterval(function, delay)`
  - 当设定好间隔时间后，function会定期执行。
- `setTimeout(function, delay)`
  - 在设定好的时间之后执行函数
- `requestAnimationFrame(callback)`
  - 告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。

## ImageData 对象

ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性：

- `width` 图片宽度，单位是像素
- `height` 图片高度，单位是像素
- `data` Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）
- `Uint8ClampedArray` 包含高度 × 宽度 × 4 bytes数据，索引值从0到(高度×宽度×4)-1

例如，要读取图片中位于第50行，第200列的像素的蓝色部份，你会写以下代码

``` js
blueComponent = imageData.data[((50 * (imageData.width * 4)) + (200 * 4)) + 2];
```

根据行、列读取某像素点的R/G/B/A值的公式：

``` js
imageData.data[((50 * (imageData.width * 4)) + (200 * 4)) + 0/1/2/3];
```

你可能用会使用Uint8ClampedArray.length属性来读取像素数组的大小（以bytes为单位）：

``` js
var numBytes = imageData.data.length;
```

### 创建一个ImageData对象

去创建一个新的，空白的`ImageData`对象，你应该会使用`createImageData()` 方法。有2个版本的`createImageData()`方法

``` js
var myImageData = ctx.createImageData(width, height);
```

上面代码创建了一个新的具体特定尺寸的ImageData对象。所有像素被预设为透明黑

你也可以创建一个被`anotherImageData`对象指定的相同像素的`ImageData`对象。这个新的对象像素全部被预设为透明黑。这个并非复制了图片数据

``` js
var myImageData = ctx.createImageData(anotherImageData);
```

## 得到场景像素数据

为了获得一个包含画布场景像素数据的ImageData对像，你可以用`getImageData()`方法：

``` js
var myImageData = ctx.getImageData(left, top, width, height);
```

这个方法会返回一个ImageData对象，它代表了画布区域的对象数据，此画布的四个角落分别表示为(left, top), (left + width, top), (left, top + height), 以及(left + width, top + height)四个点。这些坐标点被设定为画布坐标空间元素

## 在场景中写入像素数据

你可以用putImageData()方法去对场景进行像素数据的写入。

``` js
ctx.putImageData(myImageData, dx, dy);
```

dx和dy参数表示你希望在场景内左上角绘制的像素数据所得到的设备坐标。

例如，为了在场景内左上角绘制myImageData代表的图片，你可以写如下的代码：

``` js
ctx.putImageData(myImageData, 0, 0);
```

## 缩放和反锯齿

在drawImage() 方法， 第二个画布和imageSmoothingEnabled 属性的帮助下，我们可以放大显示我们的图片及看到详情内容

我们得到鼠标的位置并裁剪出距左和上5像素，距右和下5像素的图片。然后我们将这幅图复制到另一个画布然后将图片调整到我们想要的大小。在缩放画布里，我们将10×10像素的对原画布的裁剪调整为 200×200 

``` js
zoomctx.drawImage(canvas, 
                  Math.abs(x - 5), Math.abs(y - 5),
                  10, 10, 0, 0, 200, 200);
```

因为反锯齿默认是启用的，我们可能想要关闭它以看到清楚的像素。你可以通过切换勾选框来看到imageSmoothingEnabled属性的效果（不同浏览器需要不同前缀）

``` js
var img = new Image();
img.src = 'https://mdn.mozillademos.org/files/5397/rhino.jpg';
img.onload = function() {
  draw(this);
};

function draw(img) {
  var canvas = document.getElementById('canvas');
  var ctx = canvas.getContext('2d');
  ctx.drawImage(img, 0, 0);
  img.style.display = 'none';
  var zoomctx = document.getElementById('zoom').getContext('2d');
 
  var smoothbtn = document.getElementById('smoothbtn');
  var toggleSmoothing = function(event) {
    zoomctx.imageSmoothingEnabled = this.checked;
    zoomctx.mozImageSmoothingEnabled = this.checked;
    zoomctx.webkitImageSmoothingEnabled = this.checked;
    zoomctx.msImageSmoothingEnabled = this.checked;
  };
  smoothbtn.addEventListener('change', toggleSmoothing);

  var zoom = function(event) {
    var x = event.layerX;
    var y = event.layerY;
    zoomctx.drawImage(canvas,
                      Math.abs(x - 5),
                      Math.abs(y - 5),
                      10, 10,
                      0, 0,
                      200, 200);
  };

  canvas.addEventListener('mousemove', zoom);
}
```

## 保存图片

`HTMLCanvasElement`提供一个`toDataURL()`方法，此方法在保存图片的时候非常有用。它返回一个包含被类型参数规定的图像表现格式的数据链接。返回的图片分辨率是96dpi

- `canvas.toDataURL('image/png')` 默认设定。创建一个PNG图片。

- `canvas.toDataURL('image/jpeg', quality)` 创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。

当你从画布中生成了一个数据链接，例如，你可以将它用于任何`<image>`元素，或者将它放在一个有download属性的超链接里用于保存到本地

你也可以从画布中创建一个Blob对像。

- `canvas.toBlob(callback, type, encoderOptions)` 这个创建了一个在画布中的代表图片的Blob对像

## 点击区域

- `CanvasRenderingContext2D.addHitRegion()` 在canvas上添加一个点击区域。
- `CanvasRenderingContext2D.removeHitRegion()` 从canvas上移除指定id的点击区域。
- `CanvasRenderingContext2D.clearHitRegions()` 移除canvas上的所有点击区域

你可以把一个点击区域添加到路径里并检测`MouseEvent.region`属性来测试你的鼠标有没有点

``` js
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.beginPath();
ctx.arc(70, 80, 10, 0, 2 * Math.PI, false);
ctx.fill();
ctx.addHitRegion({id: "circle"});

canvas.addEventListener("mousemove", function(event){
  if(event.region) {
    alert("hit region: " + event.region);
  }
});
```

`addHitRegion()`方法也可以带一个`control`选项来指定把事件转发到哪个元素上（canvas里的元素）

``` js
ctx.addHitRegion({control: element});
```

## 焦点圈

当用键盘控制时，焦点圈是一个能帮我们在页面上快速导航的标记。要在canvas上绘制焦点圈，可以使用`drawFocusIfNeeded` 属性

- `CanvasRenderingContext2D.drawFocusIfNeeded()` 如果给定的元素获得了焦点，这个方法会沿着在当前的路径画个焦点圈。

另外，scrollPathIntoView()方法可以让一个元素获得焦点的时候在屏幕上可见(滚动到元素所在的区域)。

- `CanvasRenderingContext2D.scrollPathIntoView()` 把当前的路径或者一个给定的路径滚动到显示区域内

## [canvas 优化](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)

## 接口

- HTMLCanvasElement
- CanvasRenderingContext2D
- CanvasGradient
- CanvasPattern
- ImageBitmap
- ImageData
- TextMetrics
- Path2D