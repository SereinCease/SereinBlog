---
title: JS学习笔记
date: 2025-01-08 17:31:24
tags: JavaScript
categories: 前端
cover:
description:
---

# JS基础

## 1.输出、输入

```javascript
// 弹窗
alert('你好 js')
// 文档输出
document.write('javascript我来了!')
// 控制台打印
console.log('它~会魔法吧~')
// 输入
prompt('请输入年龄')
```

## 2.变量命名

```javascript
let num = 20
```

#### 交换值

```js
let num1 = 10
let num2 = 20
let temp
temp = num1
num1 = num2
num2 = temp
document.write(num1,num2)
```

#### 存储一组数据-使用数组

```js
let arr = ['星期一','星期二','星期三','星期四','星期五','星期六','星期日']
console.log(arr[6]);
//数组长度
console.log(arr.length);
```

## 3.常量

```js
const G = 9.8
```

## 4字符串拼接

```js
let name = '张三'
document.write('我叫' + name)
// 模板字符串
document.write(`我叫${name}，今年${age}岁`)

```

## 通过typeof关键字检测数据类型

```
console.log(typeof flag);
console.log(typeof (flag));
```

## 类型转换

```js
//隐式转换
let num1 = +prompt('请输入数字1')

//显式转换
let num = Number(prompt('请输入数字1'))
document.write(parseInt('12px'))
document.write(parseFloat('12.34px'))
```

## if分支语句

```js
if(score >= 90){
            alert('优秀')
        }else if(score >= 70 && score < 90){
            alert('良好')
        }else if(score >= 60 && score < 70){
            alert('及格')
        }else{
            alert('不及格')
        }
```



## 对象

一种数据类型，无序的数据集合

```
let goods = {
	'goods-name': '小米10青春版',
	num: 10000126,
	weight: '0.55kg',
	address: '中国大陆'
}
goods.price = 1999
// 删
delete goods.price
// 改
goods.num = 222222
// 查
console.log(goods.address)
console.log(goods['goods-name']);
```

# Web APIs





## 事件监听

```js
<button>按钮</button>

<script>
//1.获取事件源
const btn = document.querySelector('button')
//2.事件监听
btn.addEventListener('click', function(){
alert('点击了')
})
</script>
```

