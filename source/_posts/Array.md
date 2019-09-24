---
title: Array
date: 2018-05-16 16：10
tag:
 - 出云
photos:
 - http://c2.biketo.com/d/file/notes/2010-06-20/49318cf3c7dba2c1955264be5f608adb.jpg
---

<!-- 引言（简介） -->
  1. 数组介绍
  2. 数组常用方法
  3. 数组排序
  4. 数组去重、对象数组去重
<!--more-->

<!-- 详细内容 -->
##### 1. 什么是数组 
  在程序设计中，为了处理方便，把具有相同类型的若干变量按有序的形式组织起来。这些按序排列的同类数据元素的集合称为数组。
   - 数组构成：由一个或多个数组元素组成的，各元素之间使用逗号","分割。
   - 数组元素：每个数组元素由"索引下标"和"值"构成。
   - 根据维数划分为一维数组、二维数组、三维数组等多维数组。
   - 数组是一种特殊的变量，它能够一次存放一个以上的值。并且还可以通过引用索引号来访问这些值。
  
  在js中，存在一种情况，伪数组，又称类数组。是一个类似数组的对象，但是有如下几个特征。

  - 按索引方式储存数据
```js
  0: xxx, 1: xxx, 2: xxx...
```
  - 具有length属性但是length属性不是动态的，不会随着成员的变化而改变
  - 不具有数组的push()， forEach()等方法

  常见的典型伪数组，包括jQuery中通过 $() 获取的DOM元素集, 函数中的的 arguments 对象， 以及字符串String对象。

  如何判断数据是不是数组：
  ```js
  fakeArray instanceof Array === false;
  Object.prototype.toString.call(fakeArray) === "[object Object]";
  
  var arr = [1,2,3,4,6];
  arr instanceof Array === true;
  Object.prototype.toString.call(arr) === "[object Array]"
  Array.isArray()
  ```

  伪数组转化成真数组的方法:
  ```js
  var arr = [];
  for(var i = 0; i < arrLike.length; i++){
      arr.push(arrLike[i]);
  }
  或者： // 使用slice()返回一个新的数组,用call()或apply()把他的作用环境指向伪数组。
  Array.prototype.slice.call(arrLike)
  或者：
  Array.prototype.slice.apply(arrLike)
  修改原型指向
  arrLike.__proto__ = Array.prototype;
  // 这样arrLike就继承了Array.prototype中的方法，可以使用push()，unshift()等方法了，length值也会随之动态改变。 另外这种直接修改原型链的方法，还会保留下伪数组中的所有属性，包括不是索引值的属性。

  ES2015中的Array.from()方法
  // Array.from() 方法从一个类似数组或可迭代对象中创建一个新的数组实例。
  var arr = Array.from(arrLike);
  ```
  <b>总结</b>

  对象没有数组Array.prototype的属性值，类型是Object，而数组类型是Array；

  数组是基于索引的实现，length会自动更新，而对象是键值对；

#### 2.数组属性和方法
属性：
  1. constructor：
    书面说法：
    返回对创建此对象的数组函数的引用。

    解释：
    当创建一个数组时，JavaScript 会 new 一个Array构造函数，从而返回一个实例，实例中包含__proto__属性，在__proto__属性下面会包含constructor属性，此属性的值指向Array构造函数。因此，每一个对象的实例都可以通过constructor访问它的构造函数。

    示例：
    var a = [1,2,3];
    a.constructor === Array

  2. length:  数组长度

  3. prototype：
    **书面说法：**对象的原型，使您有能力向对象添加属性和方法。

    解释：
    因为Js中每个数据类型都是对象（null和undefined除外）而每个对象都继承自另一个对象，后者称之为原型对象，当构建一个对象实例时，原型上的所有方法都会被继承。

    示例：Array.prototype.name == "给数组对象添加个名称属性"
方法：
  - concat()
  书面说法：
  连接两个或多个数组，返回连接好的新数组。

  参数：
  必需-该参数可以是具体的值，也可以是数组对象。可以是任意多个。

  示例：
  var a = [1, 2, 3]
  a.concat([4, 5]) // log [1, 2, 3, 4, 5]
