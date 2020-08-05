# typescript

## 基本类型

- boolean
- null
- undefined
- number
- string
- symbol
- object
  
### 实例

``` ts 
// 定义数组
let list:number[] = [1,2,3]
// **数组泛型** Array<元素类型>
let list:Array<number> = [1,2,3]
```

## 类型断言

``` ts
// 尖括号
let onString: any = "this is a string"
let stringLength: number = (<string>oneString).length
// as
let stringLength: number = (oneString as string).length
```

## 泛型

### 泛型函数

``` ts
function hello<T>(arg: T):T {
    return arg
}
// hello<string>('hello ts')
```

### 泛型变量

``` ts
// 01
function hello<T>(arg: T[]): T[] {
    return arg
}
// 02
function hello<T>(arg: Array<T>): Array<T> {
    return arg
}
```

## 枚举（enum）

### 数字枚举

``` ts
enum obj {
    start, // 如果等于1，则end就是2，如果等于0，则end就是1
    end
}
```

### 字符串枚举
  
``` ts
enum obj {
    start = 'simon',
    end = 'jack'
}
```

## 反向映射

- 正向映射 key - value
- 反向映射 value - key
- 字符串枚举中没有反向映射

## symbol

- 可以用于对象属性的键

``` ts
const obj = {
    [symbol]: "value"
}
// console.log(obj[symbol])
```

- 常量使用symbol最的的好处，其他任何值都不可能有相同的值

## iterator 和 generator

### iterator

- 当一个对象实现了Symbol.iterator时，可以认为他是可迭代的，如array，map，set，string，int32Array，uit32Array
- for..of 迭代器返回对象的值
- for..in 迭代器返回对象的键
- ES6完全支持，ES5,3只有Array类型支持

### generator （生成器）

- function * 是用来创建generator的语法
- generator 用来创建懒迭代器
- generator 只会在调用next的时候执行
- 执行到yield 的时候会暂停并返回yield的值
- 函数在next被调用时继续执行
- 外部系统可以传递一个值到generator函数体中
- 外部系统客户抛入一个异常到generator函数体中

## 高级类型

### interface 

- 提供表达字典能力

### 交叉类型与联合类型

- 基本类型不存在交叉，如number和string不存在交叉点

``` ts
type newType = number & string
let a: newType;
interface A {
    d: number,
    z: string
}
interface B {
    f: number,
    g: string
}

type C = A & B
let c: C
```

## 类型保护与区分类型

- 联合类型可用于把值区分为不同的类型

## typeof 与 instanceof

## 类型别名

## 字面量类型

## 索引类型与映射类型

- typescript 提供了从就类型中创建新类型的一种方式，也就是映射类型，在映射类型里，新类型以相同的型是去转换就类型的每个属性

## 类型推导

## 函数

### 定义函数

### 参数

``` ts
// 可选参数
function name(firstName: string, lastName?:string) {}
// 默认参数
// 如果默认参数不是放最后则需要使用undefined 或者 null 进行占位处理
function name(firstName: string, lastName = "simon") {}
// 剩余参数
function name(firstName: string, ...names: string[]) {}
```

## 回调函数和promise

- 使用基于回调方式的异步函数时，一定不要调用两次回调，一定不要抛出错误

### 创建promise

### 订阅promise

then / catch

### promise 的链式性

### typescript 和 promise

- typescript 的强大之处是在于它可以通过promise链推测传递的值的类型

### 并行控制流

``` ts
// 多个promise 同时执行，最终可以通过all属性获取n个resolved值的数组
function userInfo(userId: string): Promise<{}> {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({userId})
        }, 1000)
    })
}

function carInfo(carId: string): Promise<{}> {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({carId})
        }, 1200)
    })
}

function goodInfo(goodId: string): Promise<{}> {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({goodId})
        }, 1500)
    })
}

Promise.all([ userInfo('1'), carInfo('2'), goodInfo('233') ]).then((res) => {
    console.log(res)
})
```

## async 和 await （ES8）

## 重载

静态类型语言常见的一种能力，简单说就是函数或者方法有相同的名称，但是参数列表不相同，这样的同名不同参数的函数或者方法之间，互相称之为重载函数或者方法

## 接口与类

### 可选属性

### 只读属性 （readonly）

- ReadonlyArray<T>类型与Array<T>类型相似只是把可变方法去掉了，因此可以确保数组创建后再也不能被修改
- 用readonly还是const最简单的判断方法是，看要把它作为变量使用还是作为一个属性，变量用const，属性用readonly

### 额外的属性检查

#### 函数类型

### 可索引类型

### 继承接口

## 类

### 定义

### 实现接口

### 继承 （extends）

### 存取器

get / set

``` ts
let passcode = 'passcode';

class Employee {
    private _fullName: string

    get fullName(): string {
        return this._fullName
    }

    set fullName(newNmae: string) {
        if (passcode && passcode == 'passcode') {
            this._fullName = newName;
        } else {
            console.log("Error: Unauthorized update of employee")
        }
    }
}

let employee = new Employee();
employee.fullName = 'Bob Smith'
if (employee.fullName) {
    alert(employee.fullName)
}
```

- 需要将编译器设置为输出ECMAScript5或更高，不支持降级到ECMAScript3
- 只有带git 不带有set 的存取器自动被判断为readonly，这在从代码生成.d.ts文件时是由帮助的，因为利用这个属性的用户会看到不允许改变它的值

### 只读属性 （readonly）

``` ts
class Octopus {
    readonly name: string
    readonly numberOflegs: number = 7
    constructor (theName: string) {
        this.name = theName
    }
}

let dad = new Octopus('wocao wuqing')
dat.name = "wuqing wocao" // name 只读，不支持修改
```

### 类函数和静态属性

static 静态属性

### 抽象类

- abstract 用于定义抽象类和在抽象类内部定义抽象方法
- 可以包含访问修饰符

### 疑问

- interface 用于约定函数，会带来哪些好处

## 命名空间与模块

### 单文件命名空间

namespace 

### 多文件命名空间

### 别名 （简化命名空间）

### 外部命名空间

## 模块

### 导出与导入

export / import

- 导出重命名

``` ts
export { name as newName }
```

- 导入重命名

``` ts
import { name as newName } from './xx'
// 将整个库到处到一个变量
import { * as newName } from './xx' 
```

- 默认导出 default

``` ts
export default class newName {}
```

- export= 和 import = require()
    - export= 定义一个模块的导出对象，类/接口/命名空间/函数/枚举
    - 如果导出了一个使用了 export= 的模块时，必须使用ts提供的特定语法`import module = require('module')`

### 生成模块

- CommonJS 服务端
- AMD 浏览器
- UMD 浏览器或服务端
- ES Module 浏览器或Node后端服务

### 外部模块

