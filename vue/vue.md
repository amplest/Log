# Vue3.0

## 快速创建项目

[传送门](https://cli.vuejs.org/zh/guide/installation.html)

``` sh
npm install -g @vue/cli
vue --version
vue create project-name
vue ui
```

## 项目配置

- Bable 会将高版本es语法转换成低版本
- Router 路由
- Vuex 状态管理
- CSS Pre-processors 预处理器
- PWA 渐进式移动应用

## 项目目录结构调整

``` sh
├── src 					# 项目主文件	
│	├── api 				# 接口			
│   ├── assets 				# 存放静态资源
│   │	├── font			# 图标字体
│ 	│	└── img 			# 图片文件
│	├── components			# 组件
│	│
│	│
│	│
│	│
│
│   ├── views				# 页面
│
│
│
│
│
│
│
│   ├── App.vue 			# 基础组件
│   ├── main.js 			# 项目入口文件，开发运行和编译都会从这个文件进，引入/编译    
│   ├── router.js 			# 路由文件    
│   ├──                  	# 状态管理
│   ├──                  
│   ├──                 
│   ├──                 
│   ├──               
│   └──           

- config 项目的一些配置文件   如果需要引入的话，直接去 store 文件里面引入即可
	 - index.js
- directive 存放Vue自定义指令
	- index.js

- lib 
	- util.js 与业务结合的工具方法 存放在 
	- tools.js 纯粹的工具方法
- router 路由文件
	- index.js 创建路由实例 / 可以添加路由守卫
	- router.js 配置路由列表 数组，包含对象

- store 状态管理
	- module 业务复杂的时候要分模块管理
	- status 
	- actions
	- index
	- mutations

- mock 请求模拟
	- index.js


```

备注：vue3.10.0之后baseUrl废除了变成了publicPath

npm install mockjs -D

## router-link / router-view

- router-link 封装了一个a标签 重要属性 to
- router-view 视图渲染组件

- 动态路由，嵌套路由
	- 嵌套路由二级路由不需要/

- 命名路由

- 命名视图 （同一个页面显示多个视图且不同位置就需要用到）

- 重定向，当前访问url重定向到另外一个url 
	- redirect 可以传入 对象，函数， 字符串

## 路由组件传参

- 布尔模式 (动态路由参数)
- 对象模式
- 函数模式 `?food=banner`

## html5 history

- hash 开发环境
- history 需要后端支持，当没有匹配到静态资源，都返回一个 index.html页面

## 导航守卫

- 跳转到结束，做逻辑处理 （如，进入页面判断是否登陆，权限控制）

- 全局守卫 router - index.js   beforeEach  afterEach beforeResolve

- 路由独享守卫 beforeEnter

- 组件内守卫，每个组件都有三个钩子(普通的有2个) beforeRouteEnter (无法使用this可以在next里面使用实例) beforeRouteLeave beforeRouteUpdate

## 完整的导航解析流程

1. 导航被触发
2. 在失活的组件（即将离开的页面组件）里调用离开守卫 beforeRouteLeave
3. 调用全局的前置守卫 beforeEach
4. 在重用的组建立调用 beforeRouteUpdate （在复用的情况下进行调用）
5. 调用路由独享的守卫 beforeEnter （路由列表配置）
6. 解析异步路由组件
7. 在被激活的组件（即将进入的页面组件）里调用beforeRouteEnter
8. 调用全局解析守卫 beforeResolve  导航确认之前，异步路由解析之后调用
9. 导航被确认
10. 调用全局的 afterEach
11. 触发DOM的更新渲染
12. 用创建好的实例调用beforeRouteEnter守卫里传给next的回调函数

## 路由元信息 meta

## 路由切换动效

transition-group

## 状态管理-1 Bus

多组件状态变得复杂的时候vuex

简单的项目使用 bus

- 父组件往子组件传值是通过属性
- 子组件往父组件传值是通过事件

- 兄弟组件

- 跨文件组件通信 bus （简单的交互用bus）

## bus注册到根实例

`Vue.prototype.$bus`

bus 创建一个空的用户实例作为交互中介

$emit 传递数据 触发当前实例的一些时间
$on 接收数据 当前事件绑定一个监听

## 状态管理-2  vuex

- state 定义对应的属性可以在全局组件中使用

computed 计算属性取值

mapState 也可以采用解构赋值取值 采用展开操作符（es6），剩余操作符，展开一个对象 可传入数组/对象


namespace = true 说明开启命名空间

- getters

es6模板语法``${state.appName}v2.0``

mapGetters

## 状态管理-3 vuex mutations

$store.commit()
使用commit修改

- mapMutations

## actions

mapActions

$store.dispatch('xxxxx','')

## moudels

可以模块包含模块

store实例也可以动态注册模块

$store.registerMoule('',{})

## vue 进阶

- 插件

- 严格模式 strict: true   (store/index)

process.env.NODE_ENV === 'development'  开发环境true

## vuex 和 双向绑定
