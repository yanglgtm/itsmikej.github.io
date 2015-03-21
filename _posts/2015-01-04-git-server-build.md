---
layout: default
title:  "Git服务搭建"
author: itsmikej
---

1. 安装git: `sudo apt-get install git`
2. 添加git用户: `sudo adduser git`
3. 创建证书登陆: 收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
4. 初始化Git仓库: 先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令: `sudo git init --bare sample.git`这样Git就会创建一个**裸仓库**(跟本地的.git目录一样)，裸仓库没有工作区,只有暂存区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。最后修改下owner: `sudo chown -R git:git sample.git`
5. 禁用 shell 登陆: 编辑`/etc/passwd` 将`git:x:1001:1001:,,,:/home/git:/bin/bash`改为`git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`
6. clone仓库: git clone git@server:/srv/sample.git
