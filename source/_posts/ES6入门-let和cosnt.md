---
title: ES6入门---let和const
date: 2018-04-16 16:34:47
tags: ES6
categories: Web开发
---

let和const均为ES6中声明变量的方法,用法类似于var,但是其所声明的变量,只在let命令所在的代码块内有效
<!-- more -->

## 没有变量提升

其所声明的变量,必须先声明再使用,若先使用,后声明则会报错

## 暂时性死区

即在存在let的块级作用域,形成了一个封闭的环境,在这其中,若先使用后声明则仍旧会报错

如下:

```JavaScript
{
  console.log(temp);//暂时性死区,会报错
  let temp //死区结束
}
```
有些隐蔽的死区如下:
```JavaScript
function foo(x=y,y=2){
  return [x,y]
}
foo() //报错
```
该报错是因为在y还没有声明的时候就使用了(将y赋值给x),此时就会报错

## 不能重复声明

在同一个作用域中,不能重复声明同一个变量
```JavaScript
function a(){
  let a = 5;
  var a = 5;
}
function b(){
  let a = 5;
  let a = 5;
}
```
这两者都会报错

## 块级作用域

let和const实际上提供了一种块级作用域
ES6允许块级作用域随意嵌套
外层作用域无法读取内层作用域的变量,内层作用域的变量可覆盖外层作用域的变量

## 在块级作用域中声明函数

```JavaScript
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
```
ES6中,理论上该函数运行会得到`I am outside`,因为外层的作用域无法访问内层变量,但内层作用域的变量可访问外层变量.
在由于兼容性问题,在浏览器中,块级作用域声明的函数,行为类似于var声明的变量

note:考虑到兼容性问题,应该避免在块级作用域中声明函数,或者声明时利用函数表达式的形式声明:
```JavaScript
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}
// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```
note: ES6 的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错

## const

const声明的变量即常量,即声明后无法改变该变量的值,所以const声明变量时候必须进行初始化,若不声明,则会报错
const与let类似:只在声明所在的块级作用域内有效,不存在变量提升,同样存在暂时性死区,只能在声明的位置后边使用

const的声明变量的值不变是针对于变量指向的内存地址不变,
对于值类型的变量类型来说,则值不能变,
但对于引用类型的变量类型来说,则该变量所指向的内存地址不变,即指向对象的指针不变,但对象的内容是可以变的,
```JavaScript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

```JavaScript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```


ES6声明变量的六种方法
`var function let const import class`

[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/04/16/ES6入门-let和const/)

