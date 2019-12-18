
# WS

## 小程序

- 列表需要做分页（触底加载）
- 无数据的时候需要给默认的暂无类型图片（可以放template里）
- 页面间切换或tabs切换需要增加loading
- showToast如果多处使用可以定义全局方法
- 山羊符号统一用`¥`

### 接口

- do: 文件名/方法名
- op: 具体方法名

### 分包中请求接口

如果后端给到的接口参数m不是本模块的话，调用接口的时候需要按照如下方式去进行调用

``` javascript
getList() {
	// goods: 接口路径里面的do
	url: 'entry/wxapp/goods',
	data:{
		op:'xxxx' //单独传
	},
	module: 'xxxx', // 填写接口返回参数m的值
	success: res => {
		console.log(res)
	}
}
```

### 页面跳转方法（封装过后）

true：清除页面栈，false：跳转不清除页面栈，可回退，中间是填写需要传的参数

``` javascript
app.util.navigateTo('xxx/xxx/xxx', {}, false);
```

### 页面触底加载思路

### 时间戳转换

思路：拿到接口后循环遍历进行转换

``` javascript
let examList = res.data.data.lists;
examList.forEach((e) => {
	e.time = app.util.formatTime(new Date(Number(e.createtime)));
	e.date = app.util.formatTime(new Date(Number(e.createtime)));
	e.time = new Date(Number(e.createtime) * 1000).toLocaleDateString();
});
```
