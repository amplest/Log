# javascript 

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
// 0
// 1
// 2
// 3
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