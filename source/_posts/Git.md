---
title: Git
date: 
tag:
 - 出云
photos:
 - https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1573478425250&di=c68957262418023fd601cfdb9f1e686f&imgtype=0&src=http%3A%2F%2Ftc.sinaimg.cn%2Fmaxwidth.800%2Ftc.service.weibo.com%2Fs5_51cto_com%2Fb9260ce2c11540a6304b74e5cd978305.jpg
---

Git 常用操作
<!--more-->
#### Git基本工作流程：
  创建分支
    git branch 分支名称
  查看分支
    git branch
  切换分支
    git checkout 分支名称
  创建并切换分支
    git checkout -b 分支名称
  分支删除
    git branch -d 分支名称
  将本地分支上传到远程服务器
    git push -u origin version0 　注：-u是--set-upstream的缩写）
  查看当前分支工作区，暂存区的工作状态
    git status   
  查看修改文件
    git diff
  提交本次修改
    git add . 
    git commit -m'本次修改备注'
  把当前的工作隐藏起来 等以后恢复现场后继续工作 
    git stash
  恢复工作现场（恢复隐藏的文件，同时删除stash列表中对应的内容）
    git stash pop