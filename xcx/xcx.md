# 小程序

## Q&A

1. :first-child/:last-child 失效
	- 如果第一个或最后一个存在兄弟节点，在小程序端是不会有效果呈现的
2. 

## 组件

- 组件可以通过`externalClasses`定义外部样式

## 配置

### js里面进行修改NavigationBarTitle
``` javasceipr
wx.setNavigationBarTitle({
	title: value.sys_title || '首页',
});
```

## WXML

## WXSS

### flex布局的坑

使用flex中的space-between的时候如果碰到最后一行是单数的情况下，中间就会有空缺，解决方案

``` html
<view class="list">
	<image src="xx"></image>
	<image src="xx"></image>
	<image src="xx"></image>
</view>
```

``` css
.list {
	display: flex;
	justify-content: space-between;
	flex-wrap: wrap;
}
.list image {
	width: 375rpx;
	height: 375rpx;
}
/*最重要的部分*/
.list:after {
	content: '';
	width: 375rpx;
}
```

### 解决switch在小程序端不能垂直居中

``` css
.wsui-flex { display: flex }
.wsui-cross--center { align-item: center }
```

``` html
<view class="wsui-flex wsui-cross--center"><switch class="wsui-flex"/></view>
```

### 文本省略号

``` css
/*多行*/
word-break: break-all;
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
/*单行*/
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
white-space: initial;
-webkit-box-orient: vertical;
-webkit-line-clamp: 1;
```

### switch-原生样式修改
``` css
/*swtich整体大小*/
.wx-switch-input{
width:82rpx !important;
height:40rpx !important;
}
/*白色样式（false的样式）*/
.wx-switch-input::before{
width:80rpx !important;
height: 36rpx !important;
}
/*绿色样式（true的样式）*/
.wx-switch-input::after{
width: 40rpx !important;
height: 36rpx !important;
}
```

### radio-原生样式修改
``` css
/*修改radio样式*/
radio, checkbox{transform:scale(1);}
radio .wx-radio-input{ /*未选中状态*/
  border-radius: 50%;
  height: 32rpx;
  width: 32rpx;
  margin-top: -4rpx;
} 
radio .wx-radio-input.wx-radio-input-checked{
  border-radius: 50%;
  height: 40rpx;
  width: 40rpx;
  line-height: 40rpx;
  margin-top: -4rpx;
  border: none;
  background: #0958db;
}
radio .wx-radio-input.wx-radio-input-checked::before{
   border-radius: 50%;/* 圆角 */
   width: 40rpx;/* 选中后对勾大小，不要超过背景的尺寸 */
   height: 40rpx;/* 选中后对勾大小，不要超过背景的尺寸 */
   line-height: 40rpx;
   text-align: center;
   font-size:28rpx; /* 对勾大小 30rpx */
   color:#fff; /* 对勾颜色 白色 */
   background: #0958db;
   transform:translate(-50%, -50%) scale(1);
   -webkit-transform:translate(-50%, -50%) scale(1);
}
```

### checkbox-原生样式修改
``` css
.checkbox .wx-checkbox-input {
	border-radius: 32rpx;
 /* 圆角 */
	width: 32rpx;
 /* 背景的宽 */
	height: 32rpx;
 /* 背景的高 */
	border: 2px solid rgba(255, 129, 92, 1);
}

.checkbox .wx-checkbox-input-disabled {
	border-radius: 32rpx;
 /* 圆角 */
	width: 32rpx;
 /* 背景的宽 */
	height: 32rpx;
 /* 背景的高 */
	background: #f3f3f3;
	border: 1px solid rgba(201, 201, 201, 1);
}

.checkbox .wx-checkbox-input.wx-checkbox-input-checked {
	background: rgba(255, 129, 92, 1);
}

.checkbox  .wx-checkbox-input.wx-checkbox-input-checked::before {
	width: 32rpx;
	height: 32rpx;
	line-height: 32rpx;
	text-align: center;
	font-size: 24rpx;
	color: #fff;
	background: transparent;
	transform: translate(-50%, -50%) scale(1);
	-webkit-transform: translate(-50%, -50%) scale(1);
}

.checkbox .wx-checkbox-input {
	border-radius: 32rpx;
 /* 圆角 */
	width: 32rpx;
 /* 背景的宽 */
	height: 32rpx;
 /* 背景的高 */
	border: 1px solid rgba(255, 129, 92, 1);
}

.checkbox .wx-checkbox-input-disabled {
	border-radius: 32rpx;
 /* 圆角 */
	width: 32rpx;
 /* 背景的宽 */
	height: 32rpx;
 /* 背景的高 */
	background: #f3f3f3;
	border: 1px solid rgba(201, 201, 201, 1);
}

.checkbox .wx-checkbox-input.wx-checkbox-input-checked {
	background: rgba(255, 129, 92, 1);
}

.checkbox  .wx-checkbox-input.wx-checkbox-input-checked::before {
	width: 32rpx;
	height: 32rpx;
	line-height: 32rpx;
	text-align: center;
	font-size: 24rpx;
	color: #fff;
	background: transparent;
	transform: translate(-50%, -50%) scale(1);
	-webkit-transform: translate(-50%, -50%) scale(1);
}
```

## JavaScript

### 三元运算符嵌套
``` js
// 01
Number(status) === 1 ? first_list : (Number(status) === 2 ? second_list : third_list)
// 02
item.package_set_type != 2 ? '最多可选' + item.num + '个':item.type.dishes + (item.type.dishes.index > 0?'，':'')
```

### picker时间格式修改成YYYY.MM.DD

``` html
<picker bindchange="bindPickerChange" value="{{xxx == '' ? '请选择' : xxx}}" mode="date" start="1900-01-01">
	<view class="picker">
      {{xxx}}
    </view>
</picker>
```

``` javascript
bindPickerChange(e) {
	let { value } = e.detail;
	let valueNew = value.split('-').join('.');
	this.setData({ 'xxx': valueNew });
},
```
