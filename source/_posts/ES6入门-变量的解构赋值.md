---
title: ES6入门---变量的解构赋值
date: 2018-04-17 09:34:49
tags: ES6
categories: Web开发
---

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构
<!--more-->

## 数组的解构赋值

```JavaScript
let [a, b, c] = [1, 2, 3]
```
可以按照对应的位置,对变量进行赋值
此种写法相当于模式匹配,例子如下:
```JavaScript
// 数组嵌套赋值
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

// 部分匹配
let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

// 部分匹配
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```
解构赋值失败,则变量的值为`undefined`
```JavaScript
let [foo] = []
let [bar, foo] = [1]
```
### 不完全匹配

```JavaScript
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

注:解构赋值的等号右边必须是可迭代的,非迭代对象将会报错

```JavaScript
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

### 解构赋值允许设定默认值

```JavaScript
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

默认对象生效的条件是该对象对应的值必须严格等于`undefined`,否则不会生效

```JavaScript
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

## 对象的解构赋值

```JavaScript
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```
变量的名称必须与对应的属性值同名才可以取到正确的值

实质上如下:先找到对象的同名属性,然后赋值给对应的变量
```JavaScript
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```

note: 
- 对象解构赋值也可以嵌套,
- 也可以指定默认值,生效条件相同,即属性值严格等于`undefined`

如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。

```JavaScript
// 报错
let {foo: {bar}} = {baz: 'baz'};
```
此时foo的值已经为`undefined`,然后又取它的子属性的值自然会报错

避免大括号在首位,如下报错是因为JavaScript引擎将其认为是代码块

```JavaScript
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error

// 正确的写法
let x;
({x} = {x: 1});
```

## 字符串的解构赋值

与数组类似

## 数值和布尔值的解构赋值

首先会将等号右边的数值或者布尔值转换为对象,然后进行解构赋值

```JavaScript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```
但`undefined`和`null`不能转换为对象,所以对他们解构赋值会报错

## 函数参数的解构赋值

```JavaScript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```
以下代码是为函数move的参数指定默认值，而不是为变量x和y指定默认值

```JavaScript
function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```

## 圆括号问题

### 不能使用圆括号的情况

1. 变量声明语句
```JavaScript
// 全部报错
let [(a)] = [1];

let {x: (c)} = {};
let ({x: c}) = {};
let {(x: c)} = {};
let {(x): c} = {};

let { o: ({ p: p }) } = { o: { p: 2 } };
```
2. 函数参数
```JavaScript
// 报错
function f([(z)]) { return z; }
// 报错
function f([z,(x)]) { return x; }
```
3. 赋值语句的模式

```JavaScript
// 全部报错
({ p: a }) = { p: 42 };
([a]) = [5];
```

### 可以使用圆括号的情况

赋值语句的非模式部分

```JavaScript
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```

## 用途

1. 交换变量的值
```JavaScript
let x = 1;
let y = 2;

[x, y] = [y, x];
```
2. 从函数返回多个值
```JavaScript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```
3. 函数参数的定义
```JavaScript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```
4. 提取JSON数据
```JavaScript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```
5. 函数参数的默认值
6. 便利Map结构
```JavaScript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
// 获取键名
for (let [key] of map) {
  // ...
}
// 获取键值
for (let [,value] of map) {
  // ...
}
```
7. 输入模块的制定方法
```JavaScript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/04/17/ES6入门-变量的解构赋值/)

