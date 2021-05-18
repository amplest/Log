# Vue

## 生命周期

`beforeCreate（创建前）`: 在数据观测和初始化事件还未开始,data、watcher、methods都还不存在，但是$route已存在，可以根据路由信息进行重定向等操作。

`created(创建后)`：在实例创建之后被调用，该阶段可以访问data，使用watcher、events、methods，也就是说 数据观测(data observer) 和event/watcher 事件配置 已完成。但是此时dom还没有被挂载。该阶段允许执行http请求操作。

`beforeMount （挂载前）`：将HTML解析生成AST节点，再根据AST节点动态生成渲染函数。相关render函数首次被调用(划重点)。

`mounted (挂载后)`：在挂载完成之后被调用，执行render函数生成虚拟dom，创建真实dom替换虚拟dom，并挂载到实例。可以操作dom，比如事件监听

`beforeUpdate`：vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改vm.data更新之后，虚拟dom重新渲染之前被调用。在这个钩子可以修改vm.data，并不会触发附加的冲渲染过程。

`updated`：虚拟dom重新渲染后调用，若再次修改$vm.data，会再次触发beforeUpdate、updated，进入死循环。

`beforeDestroy`：实例被销毁前调用，也就是说在这个阶段还是可以调用实例的。

`destroyed`：实例被销毁后调用，所有的事件监听器已被移除，子实例被销毁

创建阶段（注册实例与挂载）： beforeCreate、created、beforeMount、mounted

运行阶段：beforeUpdate、updated

注销阶段：beforeDestroy、destroyed

[生命周期图例](#smzq)

### Vue 是如何实现数据双向绑定的？

实现一个监听器 Observer：对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。

实现一个解析器 Compile：解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。

实现一个订阅者 Watcher：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。

实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。

### ajax放在vue的哪个生命周期中？

一般在，created，mounted 中都可以发送数据请求，但是，大部分时候，会在created发送请求。

Created的使用场景：如果页面首次渲染的就来自后端数据。因为，此时data已经挂载到vue实例了。

在 created（如果希望首次选的数据来自于后端，就在此处发请求）（只发了异步请求，渲染是在后端响应之后才进行的）、beforeMount、mounted（在mounted中发请求会进行二次渲染） 这三个钩子函数中进行调用。

因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。但是最常用的是在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求

优点一：能更快获取到服务端数据，减少页面 loading 时间；

优点二：放在 created 中有助于一致性，因为ssr 不支持 beforeMount 、mounted 钩子函数。

### Tab切换触发哪个生命周期

只会触发beforeUpdate、updated

## 组件

### 父子组件的生命周期顺序

加载渲染过程： 父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted 

子组件更新过程： 父beforeUpdate->子beforeUpdate->子updated->父updated

父组件更新过程： 父beforeUpdate->父updated

销毁过程： 父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

## Vuex

### 组件中如何修改vuex中state中的值

``` javascript
// 方法一
mthods: {
	...mapMutations({
		setname: 'user/SET_NAME'
	});
	setName() {
		this.setname('ax')
	}
}
// 方法二
this.$store.commit('user/SET_NAME', 'ax')
```

### Vuex中action和mutation有什么区别？

- action 提交的是mutation, 而不是直接更改状态; mutation可以直接变更状态
- action 可以包含任意一步操作; mutation只能是同步操作
- action 是用`this.$store.dispatch('ACTION_NAME',data)`来提交; mutation是用`this.$store.commit('SET_NUMBER',10)`
- 接收参数不同，mutation第一个参数是state; action第一个参数是context，其包含了
``` javascript
{
    state,      // 等同于 `store.state`，若在模块中则为局部状态
    rootState,  // 等同于 `store.state`，只存在于模块中
    commit,     // 等同于 `store.commit`
    dispatch,   // 等同于 `store.dispatch`
    getters,    // 等同于 `store.getters`
    rootGetters // 等同于 `store.getters`，只存在于模块中
}
```

### Vuex中action和mutation有什么相同点？

第二参数都可以接收外部提交时传来的参数。 `this.$store.dispatch('ACTION_NAME',data)`和`this.$store.commit('SET_NUMBER',10)`

### Vuex中action通常是异步的，那么如何知道action什么时候结束呢？

在action函数中返回Promise，然后再提交时候用then处理

``` javascript
actions:{
    SET_NUMBER_A({commit},data){
        return new Promise((resolve,reject) =>{
            setTimeout(() =>{
                commit('SET_NUMBER',10);
                resolve();
            },2000)
        })
    }
}
this.$store.dispatch('SET_NUMBER_A').then(() => {
  // ...
})
```

### 在模块中，getter和mutation接收的第一个参数state，是全局的还是模块的？

第一个参数state是模块的state，也就是局部的state。

### 在模块中，getter和mutation和action中怎么访问全局的state和getter？

- 在getter中可以通过第三个参数rootState访问到全局的state,可以通过第四个参数rootGetters访问到全局的getter。
- 在mutation中不可以访问全局的satat和getter，只能访问到局部的state。
- 在action中第一个参数context中的context.rootState访问到全局的state，context.rootGetters访问到全局的getter。

### 

## VueRoute

- 导航被触发。
- 在失活的组件里调用 `beforeRouteLeave` 守卫。
- 调用全局的 `beforeEach` 守卫。
- 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
- 在路由配置里调用 `beforeEnter。`
- 解析异步路由组件。
- 在被激活的组件里调用 `beforeRouteEnter`
- 调用全局的 `beforeResolve` 守卫 (2.5+)。
- 导航被确认。
- 调用全局的 `afterEach` 钩子。
- 触发 DOM 更新。
- 调用 `beforeRouteEnter` 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。

全局守卫: `beforeEach`/`afterEach`/`beforeResolve`

路由独享守卫: `beforeEnter`

组件内守卫: `beforeRouteEnter`/`beforeRouteUpdate`/`beforeRouteLeave`

### vue-router 路由模式有几种？

hash、history、abstract

``` javascript
switch (mode) {
  case 'history':
	this.history = new HTML5History(this, options.base)
	break
  case 'hash':
	this.history = new HashHistory(this, options.base, this.fallback)
	break
  case 'abstract':
	this.history = new AbstractHistory(this, options.base)
	break
  default:
	if (process.env.NODE_ENV !== 'production') {
	  assert(false, `invalid mode: ${mode}`)
	}
}
````

## Vue其他

### 如何获取组件高度

``` javascript
this.$refs.xxx.offsetHeight
```

### Vue首次加载页面白屏问题

- loading

- 闪烁: v-cloak: 这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕

### 请求中携带cookie

封装axios后开启`withCredentials`

## Javascript

### 基本类型&引用类型

- 常见的基本类型: Number, String, Boolean, Null, undefined
- 引用类型: Object, Array, Function, Data


### Array

- forEach, 接受三个参数: 当前值, 当前元素, 整个数组
	- 不返回值，只用来操作数据
	- 会修改原来的数组
	- forEach方法也可以接受第二个参数，绑定参数函数的this变量

``` javascript
var out = [];
[1, 2, 3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out // [1, 4, 9]
// 注意这个forEach中的第一个函数不能使用箭头函数, 因为箭头函数的this指向是上下文的作用域, 而普通函数的this才是指向当前
```
	- forEach方法无法中断执行，总是会将所有成员遍历完。
	- forEach方法不会跳过undefined和null，但会跳过空位

- map
	- 遍历数组不会改变原数,遍历完成之后会有一个新的数组生成

- filter


### ★ 闭包

栈内存 : 存储全局作用域，是JS 代码执行的环境，所有基本类型值都会在栈内存中开辟一个位置进行存储

堆内存 : 存储引用类型中的值，对象是键值对，函数是字符串。

闭包是指有权访问另外一个函数作用域中的变量的函数

- 作用
	- 1. 保护: 保护函数的私有变量不被外部干扰, 形成不销毁的栈内存
	- 2. 保存: 把一些函数内的值保存下来, 闭包可以实现方法和属性的私有化

- 劣势
	- 1. 容易导致内存泄漏。
	- 2. 闭包会携带包含其它的函数作用域，因此会比其他函数占用更多的内存。过度使用闭包会导致内存占用过多，所以要谨慎使用闭包。

- 使用场景
	- return 回一个函数
	- 函数作为参数
	- IIFE(自执行函数)
	- 循环赋值
	- 使用回调函数就是在使用闭包

- 经典面试题

``` javascript
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0]();
data[1]();
data[2]()
/* 输出
    3
    3
    3
/
// 这里的 i 是全局下的 i，共用一个作用域，当函数被执行的时候这时的 i=3，导致输出的结构都是3
```

- 形成闭包的原因: 内部的函数存在外部作用域的引用就会导致闭包。

### 原型链/作用域

### 深拷贝和浅拷贝的区别

浅拷贝：两个变量还是指向同一内存地址（复制了地址）。修改其中任意一个的值，另一个值会随之改变
``` javascript
var obj = {
    name: "li",
}
var xu = obj;
xu.name = "xu";
console.log(obj, xu);
```

深拷贝：将对象的值复制，给其新变量名及地址。两个对象修改其中任意一个的值另一个不会改变（它们是毫无关联的两个独立个体）。
``` javascript
var an = { ...obj };
an.name = "an";
console.log(obj, an);
```

### ★ 箭头函数与普通函数的区别

- 语法更加简洁、清晰
- 箭头函数没有 prototype (原型)，所以箭头函数本身没有this​​​​​​​
- 箭头函数不会创建自己的this
	- 箭头函数没有自己的this，箭头函数的this指向在定义（注意：是定义时，不是调用时）的时候继承自外层第一个普通函数的this。所以，箭头函数中 this 的指向在它被定义的时候就已经确定了，之后永远不会改变。
``` javascript
let obj = {
  a: 10,
  b: () => {
    console.log(this.a); // undefined
    console.log(this); // Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
  },
  c: function() {
    console.log(this.a); // 10
    console.log(this); // {a: 10, b: ƒ, c: ƒ}
  }
}
obj.b(); 
obj.c();
```
- call | apply | bind 无法改变箭头函数中this的指向
- 箭头函数不能作为构造函数使用
- 箭头函数不绑定arguments，取而代之用rest参数...代替arguments对象，来访问箭头函数的参数列表
- 箭头函数不能用作Generator函数，不能使用yield关键字


### 取两个数组的交集

## ES6

### 新特性

### Permission中this指向问题

### let/var/const区别 (let暂时性死区)

### 如何改变const 声明的变量

## CSS

### Flex中flex:1 和 flex: auto的区别

flex：1 的尺寸是较长的尺寸优先牺牲自己的尺寸，而flex:auto 中是较长的尺寸优先扩展自己的尺寸

### CSS.escape()

## DOM

### DOM的加载顺序

- 解析HTML结构。
- 加载外部脚本和样式表文件。
- 解析并执行脚本代码。
- 构造HTML DOM模型。
- 加载图片等外部文件。
- 页面加载完毕。

## 生命周期图例 

<span id="smzq"></span>

![`生命周期图例`](/assets/interview.png)

## vuex图例