# 小程序开发常见问题


## flex布局

该布局为上下结构,下方高度自适应

``` wxml
<view class="ms-flex ms-main--center container">
  <view class="ms-flex ms-dir--left container-1">
    <view class="container-1-top">这是第一个元素</view>
    <view class="ms-flex ms-main--center container-1-bottom">
      <view class="container-1-bottom-inner">
        <view wx:for="{{20}}" wx:key="index">这是最里面的元素</view>
      </view>
    </view>
  </view>
</view>
```

``` css
.ms-flex {
  display: flex;
}
.ms-main--center {
  justify-content: center;
}
.ms-dir--left {
  flex-direction: column;
}
.ms-cross--center {
  align-items: center;
}
.ms-cross--stretch {
  align-items: stretch;
}

.container {
  height: 100vh;
  background: #aaa;
  padding: 20rpx;
  box-sizing: border-box;
}

.container-1 {
  height: 100%;
  width: 100%;
  background-color: #bbb;
  padding: 20rpx;
  box-sizing: border-box;
}

.container-1-top {
  background-color: #ccc;
  height: 120rpx;
  padding: 20rpx;
  box-sizing: border-box;
  margin-bottom: 20rpx;
}

.container-1-bottom {
  background-color: #ccc;
  padding: 20rpx;
  height: 100%;
}

.container-1-bottom-inner {
  background: #ddd;
  width: 100%;
  height: 100%;
  overflow-y: auto;
}
```

### flex布局多行实现均分及排序

使用flex中的space-between的时候如果碰到最后一行是单数的情况下，中间就会有空缺，解决方案

``` wxml
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

## 原生样式修改

### Switch在小程序端不能垂直居中
``` css
.wsui-flex { display: flex }
.wsui-cross--center { align-item: center }
```
``` wxml
<view class="wsui-flex wsui-cross--center"><switch class="wsui-flex"/></view>
```

### Switch原生样式修改
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

### Radio原生样式修改
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

### Checkbox原生样式修改
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

## 刘海胡子兼容

``` css
padding-top: constant(safe-area-inset-top);
padding-top: env(safe-area-inset-top);
padding-right: constant(safe-area-inset-right);
padding-right: env(safe-area-inset-right);
padding-bottom: env(safe-area-inset-bottom);
padding-bottom: constant(safe-area-inset-bottom);
padding-bottom: calc(constant(safe-area-inset-bottom) + 50px);
padding-bottom: calc(env(safe-area-inset-bottom) + 50px);
padding-left: constant(safe-area-inset-left);
padding-left: env(safe-area-inset-left);
```

## WXS

### WXS中使用replace()

使用`getDate()`在IOS系统中存在兼容问题，IOS中`getDate()`只支持`2020/02/12 12:00:00`格式

``` javascript
str.replace(getRegExp('-', 'g'), '/')

// 时间对比
var timeCompare = function(subscribe_stime,start_time){
    var new_subscribe_stime = subscribe_stime.replace(getRegExp('-', 'g'), '/');
    var new_start_time = start_time.replace(getRegExp('-', 'g'), '/');

    var compare1 = getDate() > getDate(new_subscribe_stime);
    var compare2 = getDate(new_start_time) > getDate();
    var compare = compare1 && compare2 ;
    return compare;
}
```

### 兼容IOS的时间正则匹配

``` javascript
var timeCompare = function(subscribe_stime,start_time){
    var new_subscribe_stime = subscribe_stime.replace(getRegExp('-', 'g'), '/');
    var new_start_time = start_time.replace(getRegExp('-', 'g'), '/');
    var compare1 = getDate() > getDate(new_subscribe_stime);
    var compare2 = getDate(new_start_time) > getDate();
    var compare = compare1 && compare2 ;
    return compare;
}
```

## 小程序音频兼容IOS

``` wxml
 <audio controls poster="{{info.default_image}}" name="{{info.goods_name}}" author="{{info.details}}" src="{{info.audio_url}}" id="myAudio" bindtap='onAudio'></audio>
```

``` javascript
onReady() {
	this.audioCtx = wx.createAudioContext('myAudio')
},
onAudio() {
	this.setData({
	  isPlay: !this.data.isPlay
	})
	if (this.data.isPlay) {
	  this.audioCtx.play();
	} else {
	  this.audioCtx.pause();
	}
},
```

## 地理位置重复授权
``` javascript
onLoad() {
    wx.getLocation({
      type: "wgs84",
      success(res) {
        //如果首次授权成功则执行地图定位操作，具体实现代码与此文无关，就不贴出
      },
      fail: function(res) {
        //授权失败
        wx.getSetting({
          //获取用户的当前设置，返回值中只会出现小程序已经向用户请求过的权限
          success: function(res) {
            //成功调用授权窗口
            var statu = res.authSetting;
            if (!statu["scope.userLocation"]) {
              //如果设置中没有位置权限
              wx.showModal({
                //弹窗提示
                title: "是否授权当前位置",
                content:
                  "需要获取您的地理位置，请确认授权，否则地图功能将无法使用",
                success: function(tip) {
                  if (tip.confirm) {
                    wx.openSetting({
                      //点击确定则调其用户设置
                      success: function(data) {
                        if (data.authSetting["scope.userLocation"] === true) {
                          //如果设置成功
                          wx.showToast({
                            //弹窗提示
                            title: "授权成功",
                            icon: "success",
                            duration: 1000
                          });
                          wx.getLocation({
                            //通过getLocation方法获取数据
                            type: "wgs84",
                            success(res) {
                              //成功的执行方法
                            }
                          });
                        }
                      }
                    });
                  } else {
                    //点击取消按钮，则刷新当前页面
                    wx.redirectTo({
                      //销毁当前页面，并跳转到当前页面
                      url: "index" //此处按照自己的需求更改
                    });
                  }
                }
              });
            }
          },
          fail: function(res) {
            wx.showToast({
              title: "调用授权窗口失败",
              icon: "success",
              duration: 1000
            });
          }
        });
      }
    });
  }
```

## 开发实例

### 计时器

```javascript
var now = new Date()
var over = new Date(2017,4,18,20,30,00)
var time=(over.getTime())-(now.getTime())


time=time/1000;//毫秒数转秒数

setInterval (function(){

var day=parseInt(time/60/60/24)
var hour=parseInt(time/60/60%24)
var minute=parseInt(time/60%60)
var second=parseInt(time%60)
time-=1;

console.log(day+"天"+hour+"时"+minute+"分"+second+"秒")
},1000)
```

### 数组处理

``` javascript
if (res.data.errno == 0) {
  let lists = res.data.data.list;
  lists.forEach((e) => {
    e.img = e.img.split(',')
  })
  _this.setData({
    list: lists,
  })
}
// 调用使用嵌套循环，img的调用依旧是item.img但是需要设置wx:for,wx:for-item,wx:for-index
```

### 时间戳转换

```javascript
let examList = res.data.data.lists;
examList.forEach((e) => {
    e.time = app.util.formatTime(new Date(Number(e.createtime)));
    e.date = app.util.formatTime(new Date(Number(e.createtime)));
    e.time = new Date(Number(e.createtime) * 1000).toLocaleDateString();
});
```

### 根据输入框输入判断按钮状态

``` wxml
<input type="text" data-type="name" bindinput="handleInput" name="name" value="{{form.name}}" placeholder-class="input-placeholder"/>
<input type="number" data-type="mobile" bindinput="handleInput" name="mobile" value="{{form.mobile}}" placeholder-class="input-placeholder"/>
```
``` javascript
handleInput(e) {
    let _this = this;
    let { type } = e.currentTarget.dataset;
    let { value} = e.detail;
    _this.setData({
      [type]:value
    },()=>{
      let {name='',mobile=''} = _this.data;
      if (name != '' && mobile != '') {
        _this.setData({
          canSave: true
        })
      } else {
        _this.setData({
          canSave: false
        })
      }
    })
}
```

## 开发问题备注

1. :first-child/:last-child 失效
	- 如果第一个或最后一个存在兄弟节点，在小程序端是不会有效果呈现的
2. 兄弟元素间样式处理
	- :nth-of-type(2) 同级第二个兄弟元素（选择器）
3. 小程序图表组件
	- [F2 移动端可视化方案](https://f2.antv.vision/zh)/[语雀F2文档](https://www.yuque.com/antv/f2)
4. 小程序picker组件中使用fileds中的year的时候会出现显示错误，如2020年显示20
	- 给`value`默认值的时候如果是使用`new Date().getFullYear()`的话需要使用`String`方法转换下，使用`start/end`的时候也是同理(针对ios)