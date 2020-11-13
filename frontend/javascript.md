# 常用工具方法

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

## 复制

ios 9以下版本手机兼容

``` javascript
copyText(text) {
    // 数字没有 .length 不能执行selectText 需要转化成字符串
    const textString = text.toString();
    let input = document.querySelector('#copy-input');
    if (!input) {
    input = document.createElement('input');
    input.id = "copy-input";
    input.readOnly = "readOnly";        // 防止ios聚焦触发键盘事件
    input.style.position = "absolute";
    input.style.left = "-1000px";
    input.style.zIndex = "-1000";
    document.body.appendChild(input)
    }

    input.value = textString;
    // ios必须先选中文字且不支持 input.select();
    selectText(input, 0, textString.length);
    if (document.execCommand('copy')) {
    document.execCommand('copy');
        this.toast = true;
        if (this.toast) {
            setTimeout(() => {
                this.toast = false
            }, 1000);
        }
    }else {
    console.log('不兼容');
    }
    input.blur();
    // input自带的select()方法在苹果端无法进行选择，所以需要自己去写一个类似的方法
    // 选择文本。createTextRange(setSelectionRange)是input方法
    function selectText(textbox, startIndex, stopIndex) {
    if (textbox.createTextRange) {//ie
        const range = textbox.createTextRange();
        range.collapse(true);
        range.moveStart('character', startIndex);//起始光标
        range.moveEnd('character', stopIndex - startIndex);//结束光标
        range.select();//不兼容苹果
    } else {//firefox/chrome
        textbox.setSelectionRange(startIndex, stopIndex);
        textbox.focus();
    }
    }
}
```

## 下载图片

``` javascript
downloadCode() {
    var oQrcode = document.querySelector('#qrcode')
    var url = oQrcode.src
    var a = document.createElement('a')  
    var event = new MouseEvent('click')  
    // 下载图名字
    a.download = '这是下载的文件名称'
    //url 
    a.href = url 
    //合成函数，执行下载 
    a.dispatchEvent(event)
}
```

## 样式附加

``` javascript
var link = document.createElement('link');
link.rel = "stylesheet";
link.href= "/xx/xx2/public.css?v=" + new Date().getTime();
document.head.appendChild(link);
```

## 复杂数组筛选操作

主要思路就是for循环嵌套，将bigListNew的数据与listNew中的数据对比然后匹配的就拿出来形成新的数组（listNew数据是固定的）

``` javascript
onSubmit() {
    let arrFor = [];
    for (let i = 0; i<_this.bigListNew.length; i++) {
        for (let j = 0; j<_this.bigListNew[i].length; j++) {
            for (let k = 0; k<_this.listNew.table.length; k++) {
                if (_this.bigListNew[i][j].id == _this.listNew.table[k].id) {
                    arrFor.push(_this.bigListNew[i][0])
                }
            }
        } 
    }
    console.log(arrFor)
}
```