# javascript 笔记

## 变量

- 多个变量，只取最后一个被赋值的变量结果
- 【变量提升】 JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行，这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）

## 判断

- if..else

``` js
if (m !== 1) {
  if (n === 2) {
    console.log('hello');
  }
} else {
  console.log('world');
}
// world
```

- switch

``` js
switch (x) {
  case 1:
    console.log('x 等于1');
    break;
  case 2:
    console.log('x 等于2');
    break;
  default:
    console.log('x 等于其他值');
}
```

- 三元运算符 ?:

``` js
var even = (n % 2 === 0) ? true : false;
```

## 循环

- while
``` js
var i = 0;

while (i < 100) {
  console.log('i 当前为：' + i);
  i = i + 1;
}
// or
while (i < 100)  i = i + 1;
```

- for
  
- do..while

不管条件是否为真，do...while循环至少运行一次，这是这种结构最大的特点。另外，while语句后面的分号注意不要省略

``` js
var x = 3;
var i = 0;

do {
  console.log(i);
  i++;
} while(i < x);
```

## break 语句和 continue 语句

- break语句用于跳出代码块或循环

``` js
var i = 0;

while(i < 100) {
  console.log('i 当前为：' + i);
  i++;
  if (i === 10) break;
}
// or
for (var i = 0; i < 5; i++) {
  console.log(i);
  if (i === 3)
    break;
}
```

- continue语句用于立即终止本轮循环，返回循环结构的头部，开始下一轮循环

``` js
var i = 0;

while (i < 100){
  i++;
  if (i % 2 === 0) continue;
  console.log('i 当前为：' + i);
}
```

- 标签（label）

``` js
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```

## 数据类型

- 数值 number
- 字符串 string
- 布尔值 boolean
- undefined 未定义/不存在
- null 空值
- 对象 object 各种值组成的集合

原始类型：number，string，boolean

合成类型：对象

- 对象是复杂类型，可以再细分三个子类型：狭义的对象（object），数组（array），函数（function）

特殊值：undefined / null

## typeof 运算符

返回数据类型

## null，undefined，布尔值

undefined 转为数值的时候是NaN

NaN 转为数值的时候是0

undefined/null/false/0/空字符串/NaN 除此之外其余都视为true

*空对象/空数组均为true*

## 数值

- 整数/浮点数
- 数值精度：
  - 第1位：符号位，0 代表正数，1 代表负数
  - 第2位到第12，共11位：指数部分
  - 第13到第64位，共52位：小数部分
  
符号位决定了一个数的正负，指数部分决定了数值的大小，小数部分决定了数值的精度

``` js
(-1)^符号位 * 1.xx...xx * 2^指数部分
```

指数部分一共有11个二进制位，因此大小范围就是0到2047。IEEE 754 规定，如果指数部分的值在0到2047之间（不含两个端点），那么有效数字的第一位默认总是1，不保存在64位浮点数之中。也就是说，有效数字这时总是1.xx...xx的形式，其中xx..xx的部分保存在64位浮点数之中，最长可能为52位。因此，JavaScript 提供的有效数字最长为53个二进制位

精度最多只能到53个二进制位，这意味着，绝对值小于2的53次方的整数，即-2的53次方到2的53次方，都可以精确表示

``` js
Math.pow(2, 53)
// 9007199254740992

Math.pow(2, 53) + 1
// 9007199254740992

Math.pow(2, 53) + 2
// 9007199254740994

Math.pow(2, 53) + 3
// 9007199254740996

Math.pow(2, 53) + 4
// 9007199254740996
```

上面代码中，大于2的53次方以后，整数运算的结果开始出现错误。所以，大于2的53次方的数值，都无法保持精度。由于2的53次方是一个16位的十进制数值，所以简单的法则就是，JavaScript 对15位的十进制数都可以精确处理

``` js
Math.pow(2, 53)
// 9007199254740992

// 多出的三个有效数字，将无法保存
9007199254740992111
// 9007199254740992000
```

上面示例表明，大于2的53次方以后，多出来的有效数字（最后三位的111）都会无法保存，变成0

- 数值范围

根据标准，64位浮点数的指数部分的长度是11个二进制位，意味着指数部分的最大值是2047（2的11次方减1）。也就是说，64位浮点数的指数部分的值最大为2047，分出一半表示负数，则 JavaScript 能够表示的数值范围为2的1024次方到2的-1023次方（开区间），超出这个范围的数无法表示。

如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回`Infinity`

``` js
Math.pow(2, 1024) // Infinity
```

如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“负向溢出”，即 JavaScript 无法表示这么小的数，这时会直接返回0

``` js
Math.pow(2, -1075) // 0
```

``` js
var x = 0.5;

for(var i = 0; i < 25; i++) {
  x = x * x;
}

x // 0
```

上面代码中，对0.5连续做25次平方，由于最后结果太接近0，超出了可表示的范围，JavaScript 就直接将其转为0

JavaScript 提供`Number`对象的`MAX_VALUE`和`MIN_VALUE`属性，返回可以表示的具体的最大值和最小值

``` js
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324
```

- 数值的进制
  - 十进制：没有前导0的数值。
  - 八进制：有前缀0o或0O的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
  - 十六进制：有前缀0x或0X的数值。
  - 二进制：有前缀0b或0B的数值

通常来说，有前导0的数值会被视为八进制，但是如果前导0后面有数字8和9，则该数值被视为十进制

- 特殊数值
  - 正零和负零：JavaScript 内部实际上存在2个0：一个是+0，一个是-0，区别就是64位浮点数表示法的符号位不同。它们是等价的
``` js
-0 === +0 // true
0 === -0 // true
0 === +0 // true
```
  - 几乎所有场合，正零和负零都会被当作正常的0
``` js
+0 // 0
-0 // 0
(-0).toString() // '0'
(+0).toString() // '0'
```
  - 唯一有区别的场合是，+0或-0当作分母，返回的值是不相等
``` js
(1 / +0) === (1 / -0) // false
```

- NaN

需要注意的是，NaN不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number，使用typeof运算符可以看得很清楚

NaN不等于任何值，包括它本身

- Infinity （无穷）
  
Infinity有正负之分，Infinity表示正的无穷，-Infinity表示负的无穷

``` js
Infinity === -Infinity // false

1 / -0 // -Infinity
-1 / -0 // Infinity
```

Infinity与null计算时，null会转成0，等同于与0的计算

``` js
null * Infinity // NaN
null / Infinity // 0
Infinity / null // Infinity
```

Infinity与undefined计算，返回的都是NaN

``` js
undefined + Infinity // NaN
undefined - Infinity // NaN
undefined * Infinity // NaN
undefined / Infinity // NaN
Infinity / undefined // NaN
```

## 与数值相关的全局方法

- `parseInt()` 方法用于将字符串转为整数，parseInt方法还可以接受第二个参数（2到36之间）表示被解析的值的进制，返回该值对应的十进制数
- `parseFloat()` 方法用于将一个字符串转为浮点数，尤其值得注意，parseFloat会将空字符串转为NaN。这些特点使得parseFloat的转换结果不同于Number函数
``` js
parseFloat(true)  // NaN
Number(true) // 1

parseFloat(null) // NaN
Number(null) // 0

parseFloat('') // NaN
Number('') // 0

parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
```
- `isNaN()` 方法可以用来判断一个值是否为NaN
- `isFinite()` 方法返回一个布尔值，表示某个值是否为正常的数值
``` js
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```
## 字符串

反斜杠（\）在字符串内有特殊含义，用来表示一些特殊字符，所以又称为转义符

- 转义
  - `\0` ：null（\u0000）
  - `\b` ：后退键（\u0008）
  - `\f` ：换页符（\u000C）
  - `\n` ：换行符（\u000A）
  - `\r` ：回车键（\u000D）
  - `\t` ：制表符（\u0009）
  - `\v` ：垂直制表符（\u000B）
  - `\'` ：单引号（\u0027）
  - `\"` ：双引号（\u0022）
  - `\\` ：反斜杠（\u005C）
  
- 字符串与数组
  - 字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符（位置编号从0开始）
``` js
var s = 'hello';
s[0] // "h"
s[1] // "e"
s[4] // "o"

// 直接对字符串使用方括号运算符
'hello'[1] // "e"
```
  - length 属性
    - length属性返回字符串的长度，该属性也是无法改变的
``` js
var s = 'hello';
s.length // 5

s.length = 3;
s.length // 5

s.length = 7;
s.length // 5
```

- 字符集 

JavaScript 使用 Unicode 字符集。JavaScript 引擎内部，所有字符都用 Unicode 表示

- Base64 转码

JavaScript 原生提供两个 Base64 相关的方法
  - `btoa()`：任意值转为 Base64 编码
  - `atob()`：Base64 编码转为原来的值

``` js
var string = 'Hello World!';
btoa(string) // "SGVsbG8gV29ybGQh"
atob('SGVsbG8gV29ybGQh') // "Hello World!"
```

*注意，这两个方法不适合非 ASCII 码的字符，会报错*

要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法

``` js
function b64Encode(str) {
  return btoa(encodeURIComponent(str));
}

function b64Decode(str) {
  return decodeURIComponent(atob(str));
}

b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
```

## 对象

什么是对象？简单说，对象就是一组“键值对”（key-value）的集合，是一种无序的复合数据集合

对象的每一个键名又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。

``` js
var obj = {
  p: function (x) {
    return 2 * x;
  }
};

obj.p(1) // 2
```

如果属性的值还是一个对象，就形成了链式引用

``` js
var o1 = {};
var o2 = { bar: 'hello' };

o1.foo = o2;
o1.foo.bar // "hello"
```

上面代码中，对象o1的属性foo指向对象o2，就可以链式引用o2的属性

- 表达式还是语句？

对象采用大括号表示，这导致了一个问题：如果行首是一个大括号，它到底是表达式还是语句？

为了避免这种歧义，JavaScript 引擎的做法是，如果遇到这种情况，无法确定是对象还是代码块，一律解释为代码块

如果要解释为对象，最好在大括号前加上圆括号。因为圆括号的里面，只能是表达式，所以确保大括号只能解释为对象。
  
``` js
({ foo: 123 }) // 正确
({ console.log(123) }) // 报错
```

这种差异在eval语句（作用是对字符串求值）中反映得最明显

``` js
eval('{foo: 123}') // 123
eval('({foo: 123})') // {foo: 123}
```

上面代码中，如果没有圆括号，eval将其理解为一个代码块；加上圆括号以后，就理解成一个对象

- 属性的操作

读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符

``` js
var obj = {
  p: 'Hello World'
};

obj.p // "Hello World"
obj['p'] // "Hello World"
```

上面代码分别采用点运算符和方括号运算符，读取属性p

请注意，如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理

``` js
var foo = 'bar';

var obj = {
  foo: 1,
  bar: 2
};

obj.foo  // 1
obj[foo]  // 2
```

方括号运算符内部还可以使用表达式

``` js
obj['hello' + ' world']
obj[3 + 3]
```

数字键可以不加引号，因为会自动转成字符串。

``` js
var obj = {
  0.7: 'Hello World'
};

obj['0.7'] // "Hello World"
obj[0.7] // "Hello World"
```

注意，数值键名不能使用点运算符（因为会被当成小数点），只能使用方括号运算符

``` js
var obj = {
  123: 'hello world'
};

obj.123 // 报错
obj[123] // "hello world"
```

- 属性的赋值

点运算符和方括号运算符，不仅可以用来读取值，还可以用来赋值

``` js
var obj = {};

obj.foo = 'Hello';
obj['bar'] = 'World';
```

JavaScript 允许属性的“后绑定”，也就是说，你可以在任意时刻新增属性，没必要在定义对象的时候，就定义好属性

``` js
var obj = { p: 1 };

// 等价于

var obj = {};
obj.p = 1;
```

- 属性的查看 (Object.keys())

查看一个对象本身的所有属性，可以使用`Object.keys`方法

``` js
var obj = {
  key1: 1,
  key2: 2
};

Object.keys(obj);
// ['key1', 'key2']
```

- 属性的删除：delete 命令

delete命令用于删除对象的属性，删除成功后返回true

``` js
var obj = { p: 1 };
Object.keys(obj) // ["p"]

delete obj.p // true
obj.p // undefined
Object.keys(obj) // []
```

上面代码中，delete命令删除对象obj的p属性。删除后，再读取p属性就会返回undefined，而且Object.keys方法的返回值也不再包括该属性。

注意，删除一个不存在的属性，delete不报错，而且返回true

``` js
var obj = {};
delete obj.p // true
```

只有一种情况，delete命令会返回false，那就是该属性存在，且不得删除

``` js
var obj = Object.defineProperty({}, 'p', {
  value: 123,
  configurable: false
});

obj.p // 123
delete obj.p // false
```

另外，需要注意的是，delete命令只能删除对象本身的属性，无法删除继承的属性

``` js
var obj = {};
delete obj.toString // true
obj.toString // function toString() { [native code] }
```

上面代码中，toString是对象obj继承的属性，虽然delete命令返回true，但该属性并没有被删除，依然存在。这个例子还说明，即使delete返回true，该属性依然可能读取到值

- 属性是否存在：in 运算符

in运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回true，否则返回false。它的左边是一个字符串，表示属性名，右边是一个对象

``` js
var obj = { p: 1 };
'p' in obj // true
'toString' in obj // true
```

in运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。就像上面代码中，对象obj本身并没有toString属性，但是in运算符会返回true，因为这个属性是继承的

这时，可以使用对象的`hasOwnProperty`方法判断一下，是否为对象自身的属性

``` js
var obj = {};
if ('toString' in obj) {
  console.log(obj.hasOwnProperty('toString')) // false
}
```

- 属性的遍历：for...in 循环

for...in循环用来遍历一个对象的全部属性

``` js
var obj = {a: 1, b: 2, c: 3};

for (var i in obj) {
  console.log('键名：', i);
  console.log('键值：', obj[i]);
}
// 键名： a
// 键值： 1
// 键名： b
// 键值： 2
// 键名： c
// 键值： 3
```

  - for...in循环有两个使用注意点
    - 它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性。
    - 它不仅遍历对象自身的属性，还遍历继承的属性

如果继承的属性是可遍历的，那么就会被for...in循环遍历到。但是，一般情况下，都是只想遍历对象自身的属性，所以使用for...in的时候，应该结合使用hasOwnProperty方法，在循环内部判断一下，某个属性是否为对象自身的属性

``` js
var person = { name: '老张' };

for (var key in person) {
  if (person.hasOwnProperty(key)) {
    console.log(key);
  }
}
// name
```

- with 语句 

with语句的格式如下
``` js
with (对象) {
  语句;
}
```

它的作用是操作同一个对象的多个属性时，提供一些书写的方便

``` js
// 例一
var obj = {
  p1: 1,
  p2: 2,
};
with (obj) {
  p1 = 4;
  p2 = 5;
}
// 等同于
obj.p1 = 4;
obj.p2 = 5;

// 例二
with (document.links[0]){
  console.log(href);
  console.log(title);
  console.log(style);
}
// 等同于
console.log(document.links[0].href);
console.log(document.links[0].title);
console.log(document.links[0].style);
```

注意，如果with区块内部有变量的赋值操作，必须是当前对象已经存在的属性，否则会创造一个当前作用域的全局变量

``` js
var obj = {};
with (obj) {
  p1 = 4;
  p2 = 5;
}

obj.p1 // undefined
p1 // 4
```

上面代码中，对象obj并没有p1属性，对p1赋值等于创造了一个全局变量p1。正确的写法应该是，先定义对象obj的属性p1，然后在with区块内操作它。

这是因为with区块没有改变作用域，它的内部依然是当前作用域。这造成了with语句的一个很大的弊病，就是绑定对象不明确。

``` js
with (obj) {
  console.log(x);
}
```

单纯从上面的代码块，根本无法判断x到底是全局变量，还是对象obj的一个属性。这非常不利于代码的除错和模块化，编译器也无法对这段代码进行优化，只能留到运行时判断，这就拖慢了运行速度。因此，建议不要使用with语句，可以考虑用一个临时变量代替with

``` js
with(obj1.obj2.obj3) {
  console.log(p1 + p2);
}

// 可以写成
var temp = obj1.obj2.obj3;
console.log(temp.p1 + temp.p2);
```

## 函数

### 函数的声明

- function 命令

function命令声明的代码区块，就是一个函数。function命令后面是函数名，函数名后面是一对圆括号，里面是传入函数的参数。函数体放在大括号里面。

``` js
function print(s) {
  console.log(s);
}
```

上面的代码命名了一个print函数，以后使用print()这种形式，就可以调用相应的代码。这叫做函数的声明

- 函数表达式

除了用function命令声明函数，还可以采用变量赋值的写法

``` js
var print = function(s) {
  console.log(s);
};
```

种写法将一个匿名函数赋值给变量。这时，这个匿名函数又称函数表达式（Function Expression），因为赋值语句的等号右侧只能放表达式。

采用函数表达式声明函数时，function命令后面不带有函数名。如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效。

``` js
var print = function x(){
  console.log(typeof x);
};

x
// ReferenceError: x is not defined

print()
// function
```

上面代码在函数表达式中，加入了函数名x。这个x只在函数体内部可用，指代函数表达式本身，其他地方都不可用。这种写法的用处有两个，一是可以在函数体内部调用自身，二是方便除错（除错工具显示函数调用栈时，将显示函数名，而不再显示这里是一个匿名函数）。因此，下面的形式声明函数也非常常见。

``` js
var f = function f() {};
```

需要注意的是，函数的表达式需要在语句的结尾加上分号，表示语句结束。而函数的声明在结尾的大括号后面不用加分号。总的来说，这两种声明函数的方式，差别很细微，可以近似认为是等价的。


- Function 构造函数

``` js
var add = new Function(
  'x',
  'y',
  'return x + y'
);

// 等同于
function add(x, y) {
  return x + y;
}
```

上面代码中，Function构造函数接受三个参数，除了最后一个参数是add函数的“函数体”，其他参数都是add函数的参数

你可以传递任意数量的参数给Function构造函数，只有最后一个参数会被当做函数体，如果只有一个参数，该参数就是函数体。

``` js
var foo = new Function(
  'return "hello world";'
);

// 等同于
function foo() {
  return 'hello world';
}
```

Function构造函数可以不使用new命令，返回结果完全一样。

- 函数的重复声明

如果同一个函数被多次声明，后面的声明就会覆盖前面的声明

``` js
function f() {
  console.log(1);
}
f() // 2

function f() {
  console.log(2);
}
f() // 2
```

上面代码中，后一次的函数声明覆盖了前面一次。而且，由于函数名的提升（参见下文），前一次声明在任何时候都是无效的，这一点要特别注意

- 圆括号运算符，return 语句和递归

调用函数时，要使用圆括号运算符。圆括号之中，可以加入函数的参数

``` js
function add(x, y) {
  return x + y;
}

add(1, 1) // 2
```
上面代码中，函数名后面紧跟一对圆括号，就会调用这个函数

函数体内部的return语句，表示返回。JavaScript 引擎遇到return语句，就直接返回return后面的那个表达式的值，后面即使还有语句，也不会得到执行。也就是说，return语句所带的那个表达式，就是函数的返回值。return语句不是必需的，如果没有的话，该函数就不返回任何值，或者说返回undefined

函数可以调用自身，这就是递归（recursion）。下面就是通过递归，计算斐波那契数列的代码

``` js
function fib(num) {
  if (num === 0) return 0;
  if (num === 1) return 1;
  return fib(num - 2) + fib(num - 1);
}

fib(6) // 8
```

- 第一等公民

JavaScript 语言将函数看作一种值，与其它值（数值、字符串、布尔值等等）地位相同。凡是可以使用值的地方，就能使用函数。比如，可以把函数赋值给变量和对象的属性，也可以当作参数传入其他函数，或者作为函数的结果返回。函数只是一个可以执行的值，此外并无特殊之处。

由于函数与其他数据类型地位平等，所以在 JavaScript 语言中又称函数为第一等公民。

``` js
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;

// 将函数作为参数和返回值
function a(op){
  return op;
}
a(add)(1, 1)
// 2
```

- 函数名的提升

JavaScript 引擎将函数名视同变量名，所以采用function命令声明函数时，整个函数会像变量声明一样，被提升到代码头部。所以，下面的代码不会报错。

``` js
f();

function f() {}
```

表面上，上面代码好像在声明之前就调用了函数f。但是实际上，由于“变量提升”，函数f被提升到了代码头部，也就是在调用之前已经声明了。但是，如果采用赋值语句定义函数，JavaScript 就会报错。

``` js
f();
var f = function (){};
// TypeError: undefined is not a function
```

上面的代码等同于下面的形式

``` js
var f;
f();
f = function () {};
```

上面代码第二行，调用f的时候，f只是被声明了，还没有被赋值，等于undefined，所以会报错


注意，如果像下面例子那样，采用function命令和var赋值语句声明同一个函数，由于存在函数提升，最后会采用var赋值语句的定义


``` js
var f = function () {
  console.log('1');
}

function f() {
  console.log('2');
}

f() // 1
```

上面例子中，表面上后面声明的函数f，应该覆盖前面的var赋值语句，但是由于存在函数提升，实际上正好反过来

## 函数的属性和方法

### name 属性

函数的name属性返回函数的名字

### length 属性

函数的length属性返回函数预期传入的参数个数，即函数定义之中的参数个数

### toString()

函数的toString()方法返回一个字符串，内容是函数的源码

## 函数作用域

### 定义

作用域（scope）指的是变量存在的范围。在 ES5 的规范中，JavaScript 只有两种作用域：一种是全局作用域，变量在整个程序中一直存在，所有地方都可以读取；另一种是函数作用域，变量只在函数内部存在。ES6 又新增了块级作用域，本教程不涉及。

对于顶层函数来说，函数外部声明的变量就是全局变量（global variable），它可以在函数内部读取

``` js
var v = 1;

function f() {
  console.log(v);
}

f()
// 1
```

在函数内部定义的变量，外部无法读取，称为“局部变量”（local variable）

``` js
function f(){
  var v = 1;
}

v // ReferenceError: v is not defined
```

上面代码中，变量v在函数内部定义，所以是一个局部变量，函数之外就无法读取。

函数内部定义的变量，会在该作用域内覆盖同名全局变量

``` js
var v = 1;

function f(){
  var v = 2;
  console.log(v);
}

f() // 2
v // 1
```

上面代码中，变量v同时在函数的外部和内部有定义。结果，在函数内部定义，局部变量v覆盖了全局变量v

注意，对于var命令来说，局部变量只能在函数内部声明，在其他区块中声明，一律都是全局变量

``` js
if (true) {
  var x = 5;
}
console.log(x);  // 5
```

上面代码中，变量x在条件判断区块之中声明，结果就是一个全局变量，可以在区块之外读取

### 函数内部的变量提升

与全局作用域一样，函数作用域内部也会产生“变量提升”现象。var命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部

``` js
function foo(x) {
  if (x > 100) {
    var tmp = x - 100;
  }
}

// 等同于
function foo(x) {
  var tmp;
  if (x > 100) {
    tmp = x - 100;
  };
}
```

### 函数本身的作用域

函数本身也是一个值，也有自己的作用域。它的作用域与变量一样，就是其声明时所在的作用域，与其运行时所在的作用域无关

``` js
var a = 1;
var x = function () {
  console.log(a);
};

function f() {
  var a = 2;
  x();
}

f() // 1
```

上面代码中，函数x是在函数f的外部声明的，所以它的作用域绑定外层，内部变量a不会到函数f体内取值，所以输出1，而不是2。

总之，函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。

很容易犯错的一点是，如果函数A调用函数B，却没考虑到函数B不会引用函数A的内部变量

``` js
var x = function () {
  console.log(a);
};

function y(f) {
  var a = 2;
  f();
}

y(x)
// ReferenceError: a is not defined
```

上面代码将函数x作为参数，传入函数y。但是，函数x是在函数y体外声明的，作用域绑定外层，因此找不到函数y的内部变量a，导致报错。

同样的，函数体内部声明的函数，作用域绑定函数体内部。

``` js
function foo() {
  var x = 1;
  function bar() {
    console.log(x);
  }
  return bar;
}

var x = 2;
var f = foo();
f() // 1
```

上面代码中，函数foo内部声明了一个函数bar，bar的作用域绑定foo。当我们在foo外部取出bar执行时，变量x指向的是foo内部的x，而不是foo外部的x。正是这种机制，构成了下文要讲解的“闭包”现象。

## 参数

### 概述

函数运行的时候，有时需要提供外部数据，不同的外部数据会得到不同的结果，这种外部数据就叫参数

``` js
function square(x) {
  return x * x;
}

square(2) // 4
square(3) // 9
```

上式的x就是square函数的参数。每次运行的时候，需要提供这个值，否则得不到结果

### 参数的省略

函数参数不是必需的，JavaScript 允许省略参数

但是，没有办法只省略靠前的参数，而保留靠后的参数。如果一定要省略靠前的参数，只有显式传入`undefined`

### 传递方式

函数参数如果是原始类型的值（数值、字符串、布尔值），传递方式是传值传递（passes by value）。这意味着，在函数体内修改参数值，不会影响到函数外部

但是，如果函数参数是复合类型的值（数组、对象、其他函数），传递方式是传址传递（pass by reference）。也就是说，传入函数的原始值的地址，因此在函数内部修改参数，将会影响到原始值

注意，如果函数内部修改的，不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值

``` js
var obj = [1, 2, 3];

function f(o) {
  o = [2, 3, 4];
}
f(obj);

obj // [1, 2, 3]
```

上面代码中，在函数f()内部，参数对象obj被整个替换成另一个值。这时不会影响到原始值。

这是因为，形式参数（o）的值实际是参数obj的地址，重新对o赋值导致o指向另一个地址，保存在原地址上的值当然不受影响

### 同名参数

如果有同名的参数，则取最后出现的那个值

