---
title: Promise
date: 2018-09-26 17：35
tag:
 - 出云
 - es6
photos:
 - http://static.oschina.net/uploads/img/201511/11114853_S6Ed.png
---

<!-- 引言（简介） -->
  es6 - Promise理解
<!--more-->

<!-- 详细内容 -->
- promise: 是异步编程的一种解决方案，可以使异步代码看起来如同步般清晰易读，从而从回调地狱中解脱出来， promise是一个关联了执行任务的承诺，当你的任务完成时，会根据任务的成功与否，执行相关的操作。所以创建promise对象的时候，构造函数中需要传递一个函数类型的参数，来作为与此promise对象关联的任务。so promise构造函数定义 如下：
  ```js
  function Promise(resolver) {}
  ```
  - promise两个特点
    - 对象的状态不收外界影响， promise 对象的三种状态， 创建的promise对象处于pending状态，pending状态可以转换为fullfilled和rejected,只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态
      - pending promise对象处于等待状态
      - fullfilled 执行成功状态
      - rejected 执行失败状态
    - 一但状态改变，就不会再变 。 Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，能逆向转化，且转化过程只能有一次，即resolve或reject后你能再resolve和reject，即，如果你错过了它，再去监听是得不到结果的。
  - promise 用法
    - promise对象是一个 构造函数，用来生成promise实例；
    - promise 对象实例： 
    ```js
    var promise = new Promise( function(resolve, reject) {
      if (异步操作成功) {
        resolve(value);
      } else {
        reject(error)
      }
    })

    ```
    - Promise 构造函数接受一个函数做为参数，该函数的两个参数分别是resolve和reject，他们是两个函数，由JavaScript引擎提供，不用自己部署。
    - resolve函数的作用是， 将promise对象的状态从“pending”变为“resolved”，在一步操作成功时调用，并将异步操作的结果，作为参数传递出去；
    - reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从“pending”变为“rejected”),在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
    
    - promise实例生成后，可以用then方法分别指定resolve状态和rejected状态的回调函数
    ```js
    var p1 = new Promise(test)
    var p2 = p1.then(function (result) {
      console.log('成功:' + result)
    })
    var p3 = p2.catch (function (reason) {
      console.log('小姐姐不要我了', + reason)
    })

    // 变量<b>p1</b>是一个promise对象，它负责执行test函数。由于test函数在内部是异步执行的，当test函数执行成功时，我们告诉Promise对象：
  
    // 如果成功，执行这个函数

    P1.then( function (result) {
      console.log('成功:' + result)
    })

    // 当test函数执行失败时，我们告诉Promise对象

    P2.catch( function(reason) {
      console.log('小姐姐不要我了：' + reason)
    })

    // 串联起来就是

    new Promise(test).then( function (result) {
      console.log('成功：' + result)
    }).catch( function (reason) {
      console.log('小姐姐不要我了' + reason)
    })
    
    ```
    - 如果then方法返回的是一个新的Promise实例（不是原来那个Promise实例），因此可以采用链式写法，即then方法后面再调用另一个then方法
    ```js
      P1.then(function (json){
        return json;
      }).then(function(post){
        //...

      });
      //依次指定了两个回调函数，第一个回到函数完成以后会将返回结果作为参数，传入第二个回调函数。采用链式的then，可以指定一组按照次序调用的回调函数。这时前一个回调函数，有可能返回的还是一个promise对象(即有异步操作)，这时后一个回调函数，就会等待该promise对象的状态发生变化，才会被调用。
    ```
  - promise缺点 
    - 无法取消promise，一旦新建它就会立即执行无法中途取消；
    - 如果不设置回调函数， Promise内部抛出错误，不会反应到外部；
    - 当处于 pending 状态时，无法得知目前进展到那一阶段（刚开始还是即将完成）