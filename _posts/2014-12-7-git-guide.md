---
layout: default
title:  "Git常用命令笔记"
author: itsmikej
---

## Base

`git add`

`git commit`

`git status`

`git log --pretty=oneline` 查看提交日志

`git reset --hard commit_id` **回滚**,HEAD指向当前版本，上个版本就是HEAD^，上上一个版本就是HEAD^^ HEAD~!100

`git reflog` 查看每一次commit的id

`git checkout --filename` 回到最近一次git commit或git add时的状态。如果已经add，可以先回滚git reset HEAD filename，然后再git checkout -- filename

`git rm` 用于删除版本库文件

`git clone`

`git remote add origin 远程库地址` origin是远程库的名字，可更改

`git push -u origin master` 将本地库的所有内容推送到远程库上

**SSH端口修改**

* 修改URL `git remote set-url origin ssh://git@domain.com:3022/path/p1.git`
* 修改配置

```
cat>~/.ssh/config
# 映射一个别名
host newdomain
hostname domain.com
port 3022
# ctrl+D
修改p1.git项目下的git配置文件
git remote set-url origin git@newdomain:/path/p1.git
```

## 分支管理

`git checkout -b dev` checkout 加上-b参数表示创建并同切换，相当于以下两条命令：

1. `git branch dev` 创建分支
2. `git checkout dev` 切换分支

`git branch` 查看当前的分支

`git branch -d dev` 删除分支

`git merge dev` 合并dev到当前分支（Fast-forward，快速合并模式）

## 解决冲突

Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

`git log --graph` 查看分支的合并图

## 分支管理策略

`git merge --no-ff -m "merge with no-ff" dev` 表示禁用Fast forword，同时提交一个commit

每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，最后删除分支

> 一个情景是，当我们需要修复一个bug时，我们会必然想到新创建一个issue－fixbug的分支，但这时工作在dev分支上，很多代码还没有完成，但我们也不想提交。但bug必须要马上修复，还好，git提供了一个stash功能。

`git stash` 保存工作现场，然后用git status查看工作区，是干净的，修复完成之后：

`git stash list` 查看工作现场

`git stash apply` 恢复工作现场

`git stash drop` 然后删除

`git stash pop` 恢复，同时也删除

可以多次stash，恢复时用`git stash list`查看，然后用`git stash apply stash@{0}`

开发一个新的feature时，也可以跟bug同样的新建立一个分支。

## 多人协作

`git remote -v` 查看远程库信息

`git push origin master` 提交后，推送分支到远程分支上 git push origin dev

`git branch --set-upstream dev origin/dev` 指定本地dev分⽀支与远程origin/dev分⽀支的链接 (提示“no tracking information”的时候)


## 标签管理

`git tag v1.0 commit_id`打标签

`git show v1.0`

## 自定义Git

`.gitignore文件` 格式：<br/>
*.egg<br/>
*.so

`git config --global alias.st status` 配置别名

`git config --global alias.co checkout`

`git config --global alias.ci commit`

`git config --global alias.br branch`

`git config --global alias.lg "log --color --graph --
pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)
%C(bold blue)<%an>%Creset' --abbrev-commit"`

## Syncing a fork
1. `git remote add upstream 地址` 关联上游被fork的项目
2. `git fetch upstream` 取出上游分支的提交
3. `git checkout master` 却换到本地的master分支
4. `git merge upstream/master` 将上游的master分支合并到当前分支，按需解决冲突

## .gitignore格式

* /dir/* 忽略根目录的dir目录
* dir/* 忽略所有的dir目录
* *.[oa] 忽略所有.a或.o文件
* *.bak 忽略所有.bak文件
