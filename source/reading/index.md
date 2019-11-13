---
title: 阅读
date: 2019-11-11 18:23:17
---
<!-- 引言（简介） -->
<div id="xiyan">
  <label>用户名：<input id="name" type="text" /></label>
  <label>&nbsp;&nbsp;&nbsp;&nbsp;密码：<input id="pwd" type="text" /></label>
  <p id="err">账号密码不匹配</p>
  <button id="denglu">登陆</button>
</div>

<div id="list">
  <div class="list">
    <div class="list-left">
      <a href="https://bbt-fe.github.io/blog/reading/1.html">我不是最好的，却是你再也遇不到的</a>
      <p style="color: #999">2019年11月11日</p>
    </div>
    <div class="list-right"><img src="../assets/img/read_1.jpg" /></div>
  </div>

  <div class="list">
    <div class="list-left">
      <a href="https://bbt-fe.github.io/blog/reading/2.html">过你想要的生活</a>
      <p style="color: #999">2019年11月12日</p>
    </div>
    <div class="list-right"><img src="../assets/img/read_2.jpeg" /></div>
  </div>
</div>

<script>
 
  var denglu = document.getElementById('denglu')
  var list = document.getElementById('list')
  var err = document.getElementById('err')
  var xiyan = document.getElementById('xiyan')

  function login() {
    var name = document.getElementById('name').value
    var pwd = document.getElementById('pwd').value
    if( name == "" || name == undefined){
      alert("请输入用户名");
      return false;
    }
    if(pwd == "" || pwd == undefined){
      alert("请输入密码");
      return false;
    }
    if (name == 'zhouzhou' && pwd === 'xiaozhou') {
      list.style.display = 'block'
      xiyan.style.display = 'none'
    } else {
      err.style.display = 'block'
    }
  }

  denglu.onclick = function() {
    login()
  }

</script>

<style lang="scss">
  #xiyan {
    margin: 20 auto;
    text-align: center;
  }

  input {
    border: 0; 
    outline: none;
    background-color: rgba(0, 0, 0, 0);
    border-bottom: 1px solid #333;
  }

  #name{
    margin-bottom: 10px;
  }

  #err {
    color: red;
    display: none;
  }

  #list {
    display: none;
  }

  .list {
    display: flex;
    align-items: center;
    border-bottom: 1px solid #999;
  }
  .list-left {
    flex: 1;
  }
  .list-right {

  }

  img {
    width: 60px;
    height: 60px;
  }
</style>