# 日常积累

## 浏览器判断

``` javascript
if (browser.versions.mobile) {//判断是否是移动设备打开。browser代码在下面
    var ua = navigator.userAgent.toLowerCase();//获取判断用的对象
    if (ua.match(/MicroMessenger/i) == "micromessenger") {
            //在微信中打开
    }
    if (ua.match(/WeiBo/i) == "weibo") {
            //在新浪微博客户端打开
    }
    if (ua.match(/QQ/i) == "qq") {
            //在QQ空间打开
    }
    if (browser.versions.ios) {
            //是否在IOS浏览器打开
    } 
    if(browser.versions.android){
            //是否在安卓浏览器打开
    }
} else {
        //否则就是PC浏览器打开
}
```
