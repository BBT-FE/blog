---
title: 搭建博客
date: 
tag:
 - 出云 
photos:
 - https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1380429717,4199849043&fm=27&gp=0.jpg
---

hexo + github 搭建静态博客
<!--more-->

很多人都希望拥有一个方便自己分享知识、情感、感悟的地方，而博客这种东西无疑是不错的，本站采用的是 hexo + GitHub 搭建（比较简单，(⊙v⊙)），这个步骤呢，主要有下面几步

1. 环境搭建
2. 安装hexo
3. github仓库（即项目存放地址）
4. 域名（即网址，类似于-www.taobao.com-的东西）

### 环境搭建
首先呢，你要先确定你是否已经安装git和node
分别在终端中输入 git 和 node-v进行查看
如果你没有安装，那就去百度安装教程，安装完在看接下来步骤，没有准备看了也白看😕

### 安装hexo
在电脑终端或者Git中输入命令
``` 
npm install -g hexo
```
注：npm 这个是node包管理器，一般来说只要你安装node，它就存在了，

然后依次执行：

- 初始化Hexo: hexo init
- 生成静态页面：hexo generate 或者 hexo g
- 启动服务器：hexo server 或者 hexo s 
  - 注意：服务器默认是4000端口，若是安装了福昕阅读器可能端口冲突
  - 可以制定端口：hexo s -p5000

- 浏览器中访问：http://localhost:4000

### hexo主题
初次进入是属于默认的主题，你要是不喜欢呢，那只能自己配咯，
- 在themes文件夹下，可以使用别人开发好的主题，这里有很多，我使用的是这一个: https://github.com/GallenHu/hexo-theme-Daily

你需要 
```
git clone https://github.com/GallenHu/hexo-theme-Daily

```

在你的themes文件夹下

- 然后呢在 根目录下的 _config.yml 文件中 改一下theme：值
- 其他的一些功能配置你参考你自己选的主题教程就好了

当然如果你有志向，你可以自己写主题。

### 配置github仓库
首先呢，你需要一个GitHub账号，然后创建新项目
![avatar](https://github.com/BBT-FE/blog/blob/master/assets/img/repository.jpg)
项目名称建议使用 name.github.io（name = 你的github名字）
如果你电脑以前没有网GitHub上上传过任何代码，那么你需要配置ssh

在git中命令

```
cd ~
ssh-keygen
vim ~/.ssh/id_rsa.pub
```
复制里边内容到你的GitHub  settings => 的 SSH key

### 配置本地hexo

打开根目录下的——config.yml,拉倒最下面
```js
deploy:
  type: git
  repository: https://github.com/bbt-fe/bbt-fe.github.io.git (你的仓库ssh地址)
  branch: master
```

然后到根目录下 依次执行

```
hexo clean
hexo g
hexo d
```


### 指向自己的域名
 首先你需要一个域名，购买域名的地方，你可以百度，我是在阿里云买的；
 其他的你百度吧，我懒，我不想写了