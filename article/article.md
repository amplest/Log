# 技术文章

## 如何判断JS类型

前言：判断JS类型，有以下几种方法：1、typeof 2、object.property.toString.call 3、instance of。

## JS的类型
基本类型共有七种：bigInt（bigInt是一种内置对象，是除symbol外的第二个内置类型）、number、string、boolen、symbol、undefined、null。

复杂数据类型有对象（object）包括基本的对象、函数（Function）、数组（Array）和内置对象（Data等）。

## 判断JS的类型

### 方法一、typeof方法

基本数据类型除了null外都返回对应类型的字符串。

```js
typeof 1 // "number" 
typeof 'a'  // "string"
typeof true  // "boolean"
typeof undefined // "undefined"
typeof Symbol() // "symbol"
typeof 42n // "bigint"
```
注：判断一个变量是否被声明可以用（typeof 变量 === “undefined”）来判断

null返回“object”

```js
typeof null  // "object"
```
因为历史遗留的原因。typeof null 尝试返回为null失败了，所以要记住，typeof null返回的是object。

特殊值NaN返回的是 "number"
```js
typeof NaN // "number"
```
而复杂数据类型里，除了函数返回了"function"其他均返回“object”

```js
typeof({a:1}) // "object" 普通对象直接返回“object”
typeof [1,3] // 数组返回"object"
typeof(new Date) // 内置对象 "object"
```
函数返回"function"

``` js
typeof function(){} // "function"
```
所以我们可以发现，typeof可以判断基本数据类型，但是难以判断除了函数以外的复杂数据类型。于是我们可以使用第二种方法，通常用来判断复杂数据类型，也可以用来判断基本数据类型。

### 方法二、object.property.toString.call方法

他返回"[object, 类型]",注意返回的格式及大小写,前面是小写，后面是首字母大写。

基本数据类型都能返回相应的类型。

``` js
Object.prototype.toString.call(999) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(Symbol()) // "[object Symbol]"
Object.prototype.toString.call(42n) // "[object BigInt]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(true) // "[object Boolean]
```
复杂数据类型也能返回相应的类型

``` js
Object.prototype.toString.call({a:1}) // "[object Object]"
Object.prototype.toString.call([1,2]) // "[object Array]"
Object.prototype.toString.call(new Date) // "[object Date]"
Object.prototype.toString.call(function(){}) // "[object Function]"
```
这个方法可以返回内置类型

### 方法三、obj instanceof Object

可以左边放你要判断的内容，右边放类型来进行JS类型判断，只能用来判断复杂数据类型,因为instanceof 是用于检测构造函数（右边）的 prototype 属性是否出现在某个实例对象（左边）的原型链上。

```js
[1,2] instanceof Array  // true
(function(){}) instanceof Function // true
({a:1}) instanceof Object // true
(new Date) instanceof Date // true
obj instanceof Object方法也可以判断内置对象。
```

缺点：在不同window或者iframe间，不能使用instanceof。

其他方法：除了以上三种方法，还有constructor方法 和 duck type方法。

