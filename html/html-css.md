
# 常规排版

## HTML&CSS

### 第一个/最后一个以外的样式

``` css
:not(:first-child) /*除第一个以外*/
:not(:last-child) /*除最后一个以外*/
:not(:first-child)
```

### 滚动条美化
``` css
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/  
::-webkit-scrollbar  
{  
    width: 10px;  
    height: 10px;  
    background-color: #F5F5F5;  
}  
  
/*定义滚动条轨道 内阴影+圆角*/  
::-webkit-scrollbar-track  
{  
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);  
    border-radius: 10px;  
    background-color: #F5F5F5;  
}  
  
/*定义滑块 内阴影+圆角*/  
::-webkit-scrollbar-thumb  
{  
    border-radius: 10px;  
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);  
    background-color: #555;  
}  
```

### 表单
```css
/*输入框在手机端有阴影*/
input[type="text"] {
    -webkit-appearance: none;
}
textarea{
    -webkit-appearance: none;
}
/*selection用户选择样式*/
::selection
{
    color:#fff;
    background: #b6c2cc;
}

::-moz-selection
{
    color:#fff;
    background: #b6c2cc;
}
```

### 表格
```css
table {
    border: 1px solid #666666;
    border-spacing: 0;
    border-collapse: collapse;
}
table th,
table td {
    border: 1px solid #666666;
}
```

### 文本
``` css
/*倒影*/
-webkit-box-reflect: below -37px -webkit-gradient(linear,left top,left bottom,from(transparent),color-stop(10%,transparent),to(hsla(0,0%,100%,.3)));
/*溢出隐藏*/
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
word-wrap: normal;
/*CSS3属性-webkit-font-smoothing字体抗锯齿渲染*/
-webkit-font-smoothing: antialiased; 
/*none(低像素文本友好) | subpixel-antialiased(默认) | antialiased*/
-moz-osx-font-smoothing: grayscale;/*inherit | grayscale*/
```

### 图片
```css
/*垂直居中*/
position: absolute;
top: 0;
bottom: 0;
left: 0;
right: 0;
max-width: 100%;
max-height: 100%;
display: block;
vertical-align: middle;
text-align: center;
margin: auto;
```

### 浮动
```css
/*方案一*/
.clearfix:after{
    content:".";
    display:block;
    height:0;
    clear:both;
    visibility:hidden
}
.clearfix{*+height:1%;}
/*方案二-bootstarp*/
.clearfix:before,
.clearfix:after {
    display: table;
    content: " ";
}
.clearfix:after {
    clear: both;
}
/*方案三（源于国外）*/
.clearfix{ 
    overflow:auto;
    _height:1%
}
/*方案四（源于端友）*/
.clearfix{
    overflow:hidden;
    _zoom:1;
}
/*使用双伪元素清除浮动*/
.clearfix:before,
.clearfix:after {
  content: "";
  display: block;
  clear: both;
}
.clearfix {
      zoom: 1;
}
/*比较严谨的一种做法*/
.clearfix:after {
  content:"";
  height:0;
  line-height:0;
  display:block;
  visibility:hidden;
  clear:both;
}
.clearfix {
  zoom:1;
}
```

### box-shadow
hover的时候让投影当边框使用
```css
a:hover {
  -webkit-box-shadow: 0 0 0 1px #ff5c01, 0 2px 15px 0 rgba(251, 123, 51, 0.3);
  -moz-box-shadow: 0 0 0 1px #ff5c01, 0 2px 15px 0 rgba(251, 123, 51, 0.3);
  box-shadow: 0 0 0 1px #ff5c01, 0 2px 15px 0 rgba(251, 123, 51, 0.3);
}
```

## flex

### flex 父元素设置
<table>
    <tbody>
        <tr>
            <td width="150">
                属性
            </td>
            <td>
                值
            </td>
            <td>方向</td>
            <td>起始位置/备注</td>
        </tr>
        <tr>
            <td rowspan="4">
                flex-direction<br/>控制项目方向
            </td>
            <td>
                row
            </td>
            <td>
                横向排列：左-&gt;右
            </td>
            <td>
                left
            </td>
        </tr>
        <tr>
            <td>
                row-reverse
            </td>
            <td>
                横向排列：右-&gt;左
            </td>
            <td>
                right
            </td>
        </tr>
        <tr>
            <td>
                column
            </td>
            <td>
                垂直排列：上-&gt;下
            </td>
            <td>
                top
            </td>
        </tr>
        <tr>
            <td>
                column-reverse
            </td>
            <td>
                垂直排列：下-&gt;上
            </td>
            <td>
                bottom
            </td>
        </tr>
        <tr>
            <td rowspan="3">
                flex-wrap<br/>控制项目换行
            </td>
            <td>
                nowrap（默认）
            </td>
            <td>
                不换行
            </td>
            <td></td>
        </tr>
        <tr>
            <td>
                wrap
            </td>
            <td>
                换行，第一行在上
            </td>
            <td></td>
        </tr>
        <tr>
            <td>
                wrap-reverse
            </td>
            <td>
                换行，第一行在下
            </td>
            <td></td>
        </tr>
        <tr>
            <td>
                flex-flow
            </td>
            <td>
                flex-direction | flex-wrap
            </td>
            <td>
                对应为row ，nowrap
            </td>
            <td></td>
        </tr>
        <tr>
            <td rowspan="6">
                justify-content<br/>项目在主轴的对齐方式
            </td>
            <td>
                flex-start
            </td>
            <td>
                左对齐
            </td>
            <td>
                left
            </td>
        </tr>
        <tr>
            <td>
                flex-end
            </td>
            <td>
                右对齐
            </td>
            <td>
                left
            </td>
        </tr>
        <tr>
            <td>
                center
            </td>
            <td>
                居中对齐
            </td>
            <td></td>
        </tr>
        <tr>
            <td>
                space-btween
            </td>
            <td>
                两端对齐中间间距相等
            </td>
            <td></td>
        </tr>
        <tr>
            <td>
                space-around
            </td>
            <td>
                子元素中间间距是两侧距边框距离的一倍
            </td>
            <td></td>
        </tr>
        <tr>
            <td>
                space-evenly
            </td>
            <td>
                子元素中间间距与两侧距边框距离相等
            </td>
            <td></td>
        </tr>
        <tr>
            <td rowspan="5">
                align-items<br/>项目在交叉轴的对齐方式
            </td>
            <td>
                flex-start
            </td>
            <td>
                交叉轴的起点对齐：上-&gt;下
            </td>
            <td rowspan="5">
                如果单根轴线的时候<br/>align-items<br/>如果多根轴线的时候<br/>align-content<br/>二者不能并存
            </td>
        </tr>
        <tr>
            <td>
                flex-end
            </td>
            <td>
                交叉轴的终点对齐：下-&gt;上
            </td>
        </tr>
        <tr>
            <td>
                center
            </td>
            <td>
                交叉轴的中点对齐
            </td>
        </tr>
        <tr>
            <td>
                baseline
            </td>
            <td>
                文字基线对齐（文字下边对齐一条线）
            </td>
        </tr>
        <tr>
            <td>
                stretch（默认）
            </td>
            <td>
                如果子元素未设置高度，则自动填充满父元素容器高度
            </td>
        </tr>
        <tr>
            <td rowspan="6">
                align-content<br/>项目多条轴线的对齐方式
            </td>
            <td>
                flex-start
            </td>
            <td colspan="2">
                交叉轴的起点对齐
            </td>
        </tr>
        <tr>
            <td>
                flex-end
            </td>
            <td colspan="2">
                交叉轴的终点对齐
            </td>
        </tr>
        <tr>
            <td>
                center
            </td>
            <td colspan="2">
                交叉轴的中点对齐
            </td>
        </tr>
        <tr>
            <td>
                space-between
            </td>
            <td colspan="2">
                交叉轴两端对齐，中间间距相等
            </td>
        </tr>
        <tr>
            <td>
                space-around
            </td>
            <td colspan="2">
                交叉轴元素间距是两侧距离边框间距的一倍
            </td>
        </tr>
        <tr>
            <td>
                stretch（默认）
            </td>
            <td colspan="2">
                如果子元素未设置高度，则自动填充满父元素容器高度
            </td>
        </tr>
    </tbody>
</table>

### flex 子元素设置
<table>
    <tbody>
        <tr>
            <td width="150">
                order
            </td>
            <td>
                0（默认）
            </td>
            <td colspan="2">
                默认为0，填写整数，值越小越靠前
            </td>
        </tr>
        <tr>
            <td rowspan="2">
                flex-grow
            </td>
            <td rowspan="2">
                0（默认）
            </td>
            <td colspan="2">
                如果元素的flex-grow都为1的话，则他们将等分剩余空间
            </td>
        </tr>
        <tr>
            <td colspan="2">
                如果有一个元素的flex-frow为2，其余都为1的话，则这个元素占据的剩余空间比其他元素多一倍
            </td>
        </tr>
        <tr>
            <td rowspan="2">
                flex-shrink
            </td>
            <td rowspan="2">
                1（默认）
            </td>
            <td colspan="2">
                元素缩小比例，所有元素flex-shrink设置为1的话，如果空间不足，项目将都缩小
            </td>
        </tr>
        <tr>
            <td colspan="2">
                如果一个项目flex-shrink为0，其余都为1，空间不足时为0的不缩小，其余缩小
            </td>
        </tr>
        <tr>
            <td>
                flex-basis
            </td>
            <td>
                auto（默认）
            </td>
            <td colspan="2">
                项目占据主轴空间，auto-&gt;元素原来大小
            </td>
        </tr>
        <tr>
            <td>
                flex
            </td>
            <td>
                flex-grow | flex-shrink | flex-basis
            </td>
            <td colspan="2">
                快捷值：auto（1，1，auto），none（0，0，auto）
            </td>
        </tr>
        <tr>
            <td rowspan="6">
                align-self
            </td>
            <td>
                auto
            </td>
            <td  colspan="2">
                继承父元素align-items，如果没有父元素，则等同于stretch
            </td>
        </tr>
        <tr>
            <td>
                flex-start
            </td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>
                flex-end
            </td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>
                center
            </td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>
                baseline
            </td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>
                stretch
            </td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>
