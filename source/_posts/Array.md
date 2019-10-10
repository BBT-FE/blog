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

  - join()
  书面说法：
  把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。

  参数：
  separator(分隔符)：可选-分隔符，当省略时，默认使用逗号分隔。

  示例：
  ```js
  var a = [1, 2, 3]; a.join(",") // log "1, 2, 3"
  ```

  - pop()
  书面说法：
  删除并返回数组的最后一个元素

  参数：
  无

  示例：
  ```js
  var a = [1, 2, 3]; a.pop() // log "3"
  ```

 - push()
  书面说法：
  向数组的末尾添加一个或更多元素，并返回新的长度。

  参数：
  可以使对象，可以使字符串，该方法会直接修改原数组，返回修改后的数组长度。

  示例：
  ```js
   var a = [1,2,3]
   var info = { name: 'zxiyan' } : var info = 'xiyan'
   a.push(info)
   console.log(a) // [1,2,3,{'xiyan'}] : [1,2,3,'xiyan']
  ```

 - shift()
  书面说法：
  删除并返回数组的第一个元素

  参数：
  无

  示例：
  ```js
  var a = [1, 2, 3];
  a.shift() // log "1"
  console.log(a) // [2, 3]
  ``` 

  - unshift()
  书面说法：
  向数组的开头添加一个或更多元素，并返回新的长度

  参数：
  可以使对象，可以使字符串，该方法会直接修改原数组，返回修改后的数组长度。

  示例：
  ```js
  var a = [1,2,3]
  var info = { name: 'zxiyan' } : var info = 'xiyan'
  a.unshift(info)
  console.log(a) // [{}, 1,2,3]
  ```

  - slice()
  书面说法：
  从某个已有的数组返回选定的元素

  参数：
  start：必需-要选取的开始位置的下标（当为负数时，则从数组末尾开始算，-1则为最后一个，-2为倒数第二个，依此类推）
  end：可选-要选取的结束位置的下标（当为负数时，同上。当没有指定时，默认下标为最后一个元素。）

  示例：
  ```js
  var a = [1, 2, 3]; 
  a.slice(0,2) // log [1, 2] 
  a.slice(0,-1) // log [1, 2] 
  a.slice(-3,-1) // log [1, 2]
  ```

  - splice(index, howmany, item-n)
  书面说法：
  删除元素，并向数组添加新元素。

  参数：
  index：必需-整数，规定添加/删除的位置，如为负数则从结尾处开始计算。
  howmany：必需-要删除的元素数量，如为0，则不删除元素。
  item-n：可选-向数组中添加的新元素。当 howmany 为0时，新元素就增加至 index 处前面，否则将替换从 index 到 howmany 个元素的位置。

  示例：
  ```js
  var months = ['Jan', 'March', 'April', 'June'];
  months.splice(1, 0, 'Feb'); // Array ['Jan', 'Feb', 'March', 'April', 'June']
  months.splice(2, 1); // Array ["Jan", "March", "June"]
  months.splice(1); // Array ["Jan"]
  ```
  - reverse()
  书面说法：
  方法将数组中元素的位置颠倒，并返回该数组。该方法会改变原数组。

  参数：
  无

  示例：
  ```js
  var array = ['one', 'two', 'three'];
  array.reverse()
  console.log(array);//  Array ["three", "two", "one"]
  ```

  - sort()
  书面说法：
  方法用原地算法对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列时构建的
  由于它取决于具体实现，因此无法保证排序的时间和空间复杂性

  参数：
  无

  示例：
  ```js
  var months = ['March', 'Jan', 'Feb', 'Dec'];
  months.sort();
  console.log(months);
  // expected output: Array ["Dec", "Feb", "Jan", "March"]

  var array1 = [1, 30, 4, 21, 100000];
  array1.sort();
  console.log(array1);
  // expected output: Array [1, 100000, 21, 30, 4]

  ```

  - toSource()	
  书面说法：
  返回一个字符串,代表该数组的源代码.
  非标准
  该特性是非标准的，请尽量不要在生产环境中使用它！

  参数：
  无

  示例：
  ```js
  var alpha = new Array("a", "b", "c");
  alpha.toSource();   //返回["a", "b", "c"]
  ```
  - toString()	
  书面说法
  把数组转换为字符串，并返回结果。

  参数：
  无

  示例：
  ```js
  var array1 = [1, 2, 'a', '1a'];
  console.log(array1.toString());
  // expected output: "1,2,a,1a"
  ```
  - toLocaleString()	// 这个有点搞不懂，待理解
  书面说法：
    把数组转换为本地数组，并返回结果。
  参数：
  无

  示例：
  ```js
  
  ```
  - valueOf()
  书面说法：
  方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
  首个被找到的元素在数组中的索引位置; 若没有找到则返回 -1
  参数：
  需要查找的元素，或者 全部

  示例：
  ```js
  var array = [2, 5, 9];
  array.indexOf(2);     // 0
  array.indexOf(7);     // -1
  ```
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array


#####   4. 数组去重、对象数组去重
  - 数组去重
    - 利用新数组
      ```js
      function uniqueArray(arr) {
      var temp = [];
      for (var i = 0; i < arr.length; i++) {
        if (temp.indexOf(arr[i]) === -1) {
        // if (arr.indexOf(arr[i]) === i) {
          temp.push(arr[i]);
        }
      }
      return temp;
      }
      ```
    - 对原数组splice
    ```js
    function uniqueArray(arr) {
      for (var i = 0; i < arr.length; i++) {
        if (arr.indexOf(arr[i]) != i) {
            arr.splice(i,1);    //删除重复元素
            i--;    //数组下标回退
        }
      }
      return arr;
    }
    ```
    - 对原数组filter
    ```js
    function uniqueArray(arr) {
      return arr.filter((item, index, thisArr) => {
        return thisArr.indexOf(item) === index;
      })
    }
    ```
    - 利用Set
    ```js
    function uniqueArray(arr) {
    return Array.from(new Set(arr));
    //return [...new Set(arr)];  使用扩展运算符效果一致
    }
    ```
    - 利用简单对象映射（缺点：无法正确识别数组元素为"1"与1的区别）
    ```js
    function uniqueArray(arr) {
      var map = {},
        ret = [];
      for (var i = 0; i < arr.length; i++) {
        var item = arr[i];
        if (!map[item]) {
          map[item] = true;
          ret.push(item);
        }
      }
      return ret;
    }
    ```
    - 针对以上做法缺点，可以将普通对象改为Map对象
    ```js
    function uniqueArray(arr) {
      var map = new Map(),
          ret = [];
      for (var i = 0; i < arr.length; i++) {
        var item = arr[i];
        if (!map.get(item)) {
          map.set(item, true);
          ret.push(item);
        }
      }
      return ret;    
    }
    ```
    - 利用sort（缺点：数组顺序被打乱）
    ```js
    function uniqueArray(arr) {
      arr.sort();
      var temp = arr[0];
      for (var i = 1; i < arr.length; i++) {
        if (arr[i] == temp) {
          arr.splice(i, 1); //删除重复元素
        } else {
          temp = arr[i];
        }
      }
      return arr;
    }
    ```

  - 对象数组去重
  ```js
   // list查重整合数据 id 数组中唯一值进行对比
    repeatCheck(arr) {
      // 先进行去重
      var result = []
      var obj = {}
      for (var i = 0; i < arr.length; i++) {
        if (!obj[arr[i].id]) {
          result.push(arr[i])
          obj[arr[i].id] = true
        }
      }

      // 去重之后的数组和原数组进行比较赋值
      result.forEach((item, index) => {
        let name = ''
        arr.forEach((i, o) => {
          if (item.id === i.id) {
            name = i.goodsName + ',' + name  // 这里进行前后数组数据进行合并
          }
        })
        item.goodsName = name.split(',')
      })
      arr = result
    },
    ```

    