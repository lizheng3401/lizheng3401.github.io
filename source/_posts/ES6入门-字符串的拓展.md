---
title: ES6入门-字符串的拓展
date: 2018-04-18 09:43:56
tags: ES6
categories: Web开发
---

JavaScript中字符串使用unicode表示法,一般由2个字节构成一个字符,超过该范围的字节会以8个字节形式给出 
<!--more -->

## codePointAt()

```JavaScript
var s = "𠮷";

s.length // 2
s.charAt(0) // ''
s.charAt(1) // ''
s.charCodeAt(0) // 55362
s.charCodeAt(1) // 57271

let s = '𠮷a';

s.codePointAt(0) // 134071
s.codePointAt(1) // 57271

s.codePointAt(2) // 97
```

## String.fromCodePoint()

```JavaScript
//从码点返回对应的字符
String.fromCodePoint(0x20BB7)
// "𠮷"
String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
// true

```
若传入多个参数,则该函数将会合并所有参数,形成一个字符串返回

## 字符串的遍历接口

使用`for....of`可以循环遍历,并且他可以识别大于0xffff的码点,

```JavaScript
for(let it of "ABC"){
  console.log(it);
}
// "A"
// "B"
// "C"
let str = "𠮷"
for(let i of str){
  console.log(i)
}
```

## at()

返回字符串指定位置的字符,但无法识别大于0xffff的字符

```JavaScript
"abc".charAt(0) // "a"
"𠮷".charAt(0) // "\uD842"
```

## normalize()

Unicode正规化:用来将字符的不同表示方法统一为同样的形式

## includes(),startsWith(),endsWith()

`includes()`: 返回布尔值,表示是否找到了参数字符串
`startsWith()`: 返回布尔值,表示参数字符串是否在源字符串的头部.
`endsWith()`:返回布尔值,表示参数字符串是否在源字符串的尾部.

```JavaScript
let s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```
可利用第二个参数指定开始搜索的范围
```JavaScript
let s = 'Hello world!';

s.startsWith('world', 6) // true 从第六个字符开始到结尾组成的字符串是否以`world`结尾
s.endsWith('Hello', 5) // true 前五个字符组成的字符串是否以`Hello`结尾的
s.includes('Hello', 6) // false 从第六个字符开始到结尾组成的字符串是否包含`Hello`
```

## repeat()

返回一个新的字符串,将对应字符串重复n次
```JavaScript
"x".repeat(3) // "xxx"
```
首先会对参数进行四舍五入,完成了若得到了除0或者正整数之外的值,则会报错或返回空字符串

## padStart(),padEnd()

用户字符串的补全,分别为首部补全,尾部补全,常见于数字的格式化时候的补全
例如:十位数的字符串
```JavaScript
'1'.padStart(10, '0') // "0000000001"
'12'.padStart(10, '0') // "0000000012"
'123456'.padStart(10, '0') // "0000123456"
```
参数1为显示指定返回的字符串的长度,参数2默认为空格,即空格补全,
若补全长度之和大于指定的长度,则截取补全的字符以满足指定长度.
```JavaScript
'x'.padEnd(4, 'ab') // 'xaba'
```

## matchAll()

返回一个字符串对该正则表达式的所有匹配

## 模板字符串

利用反引号标识,可嵌入变量,可定义多行字符串
```JavaScript
let a = "1234"
let str = `Hello${a}`
console.log(str) // "Hello1234"

// 多行字符串

$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`);

```

[欢迎访问我的博客了解更多](http://lizheng3401.github.io/2018/04/18/ES6入门-字符串的拓展/)

