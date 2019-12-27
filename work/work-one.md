
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

### 仿微信通讯录

最近需要些一个学生列表根据字母进行区分排序

后端返回数据结构，后端需要帮助拿到首字母，及数据返回按照字母A-Z的顺序排序
``` json
{
    "errno": 0,
    "message": "success",
    "data": [
        {
            "id": "2",
            "sname": "阿丑",
            "avatar": "",
            "sign": "A"
        },
        {
            "id": "1",
            "sname": "云安",
            "avatar": "",
            "sign": "Y"
        },
        {
            "id": "3",
            "sname": "元智",
            "avatar": "",
            "sign": "Y"
        }
    ]
}
```
前端代码结构
``` html
<view class="list">
  <view id="sign-box{{index}}" class="sign-box" wx:for="{{list}}" wx:key="index">
    <!-- 首字母 -->
    <view class="wsui-flex wsui-main--between month sign">
      <view>{{item.sign}}</view>
    </view>
    <!-- 内容区域 -->
    <block wx:for="{{item.list}}" wx:key="list_i" wx:for-item="list" wx:for-index="list_i">
      <view class="wsui-flex wsui-main--between wsui-cross--center box {{list_i === item.list.length-1 ? 'mb0' : ''}}">
        <view class="wsui-flex wsui-cross--center">
          <image class="avatar" src="{{list.avatar || '/teacher/images/tx.png'}}"></image>
          <view class="name">{{list.sname || '学生名字'}}</view>
        </view>
      </view>
    </block>
  </view>
</view>
<!-- 右侧字母区域A-Z -->
<view class="wsui-flex wsui-main--center wsui-cross--center letters">
  <view>
    <view class="letter {{index === letterNum ? 'active' : ''}}" wx:for="{{list}}" wx:key="index" bindtap="getLetter" data-i="{{index}}" hover-class="wsui-hover__rect">
      {{item.sign}}
    </view>
  </view>
</view>
```
前端js代码
``` javascript
data: {
	id: '',
	list: [],
    letterNum: 0,
    topArr: ''
},
getMemberList(){
    let _this = this;
    let {id} = _this.data;
    util.request({
      url: 'xxx',
      data: {
        id
      },
      success: (res) => {
      	// 这里的变量赋值不是很明白 
        let t = res.data, flag, list;
        if (t.errno === 0 && t.data.length) {
          list = this.sortList(t.data);
        } else {
          list = [];
        };
        this.setData({ list }, () => {
           this.getHeight();
        });
      },
    })
  },
sortList(list) {
	// 重点代码
	let arr = [], sign = '';
	list.length && list.forEach(v => {
	  if (sign === v.sign) {
	    arr.forEach((item, index) => {
	      if (item.sign === sign) arr[index].list.push(v);
	      return
	    });
	  } else {
	    let json = {
	      sign: v.sign,
	      list: []
	    };
	    sign = v.sign;
	    json.list.push(v);
	    arr.push(json);
	  }
});
	return arr;
},
getLetter(e) {
	let { i } = e.currentTarget.dataset;
	let { topArr } = this.data;
	wx.pageScrollTo({
	  scrollTop: topArr[i]
	});
},
getHeight() {
	let query = wx.createSelectorQuery();
	let that = this;
	query.selectAll('#header, .sign-box').boundingClientRect();
	query.exec(function (res) {
	  let headerH = res[0][0].height;
	  let arr = res[0].slice(1);
	  let topArr = [];
	  arr.length && arr.forEach((v, i) => {
	    topArr.push(v.top - headerH);
	  });
	  that.setData({ topArr, headerH });
	});
},
```

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

``` javascript
page({
	// 定义初始值
	data: {
		page: 1,
		psize: 10,
		total: 0,
		list:[]
	},
	// 进入页面重置列表后再请求接口
	onShow() {
		this.setData({
		page:1,
		list: [],
		},()=>{
			this.getList()
		})
	},
	// 接口请求
	getList() {
	    let that = this;
	    let {page,psize,total,list} = that.data;
	    app.util.request({
	      url: 'entry/wxapp/api',
	      data: {
	        page,
	        psize
	      },
	      success(res) {
	        if (res.data.errno === 0) {
	          let examList = res.data.data.lists;
	          let total = res.data.data.total;
	          examList.forEach((e) => {
	            e.time = app.util.formatTime(new Date(Number(e.createtime)));
	          });
	          list = [...list, ...examList];
	          that.setData({
	            list,total
	          });
	        } else {
	          wx.showToast({
	            title: '出错啦！',
	            icon: 'none',
	          });
	        }
	      },
	      fail(err) {
	        that.setData({
	          list: [],
	        });
	      },
	    });
	  },
	// 触底事件
	onReachBottom() {
	    let { page, psize, total } = this.data;
	    if (page < Math.ceil(total / psize)) {
	      this.setData({
	        page: ++page
	      }, () => {
	        this.getExamList();
	      });
	    };
	  },
})
```

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

### 后端给的数据img需要转成数组显示

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

### input输入控制按钮点击

``` html
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
},
```