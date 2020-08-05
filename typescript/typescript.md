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
// 数组泛型 Array<元素类型>
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
```