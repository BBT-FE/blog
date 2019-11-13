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

    1. git status
命令用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被Git tracked到。git status不显示已经commit到项目历史中去的信息。
2. git add <file>
命令将文件内容添加到索引(将修改添加到暂存区)。也就是将要提交的文件的信息添加到索引库中。
3. git add . 
命令将所有的修改文件上传到暂缓区。
4. git commit -m'修改记录'
推送修改到本地库中。
5. git pull
服务器上的仓库中代码拉到本地
6. git push 
将本地仓库修改推送到服务器上的仓库中
7. git merge
合并分支
在本地修改代码add 过后没有提交成功，当前修改代码丢失时执行（8，9条命令）
8. git reset --hard HEAD~1
回退到上一个版本
9. git fsck --lost-found
执行该命令

10. git log
查看提交历史

11. git remote add origin 地址
本地库关联远程仓库

12. git stash 
可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作。

13. git stash pop
释放stash暂存

14. git diff
命令用于显示提交和工作树等之间的更改。此命令比较的是工作目录中当前文件和暂存区域快照之间的差异,也就是修改之后还没有暂存起来的变化内容。

15. 合并分支部分文件
  现有A、B两个分支，需要把B分支下的某个文件合并到A分支，执行命令

```
git checkout A
git checkout B <file>(文件路径位置)
```