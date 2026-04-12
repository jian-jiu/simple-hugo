---
title: git
date: 2026-04-12
slug: git
categories:
    - git
tags:
    - git
---

[规范参考](https://blog.csdn.net/Run_ning_/article/details/95939643)

# git历史合并（Simple）
拉取项目删除git
创建新的git并且推送
合并远程
```shell
控制台找到命令
加上 --allow-unrelated-histories
git -c credential.helper= -c core.quotepath=false -c log.showSignature=false merge yc/main --allow-unrelated-histories
```
解决冲突
git commit
:q 退出
可以推送了
## Git

搜索

in:name springboot stars:>4000 pushed:>2020-01-01 language:java  forks:>1000

## 证书问题
```shell
git config --get http.sslCAInfo

git config http.sslCAInfo "D:/must/Git/mingw64/etc/ssl/certs/ca-bundle.crt"

git config --system http.sslCAInfo "D:/must/Git/mingw64/etc/ssl/certs/ca-bundle.crt"
```
## 初始化仓库

git init

2)、设置Git签名：
git config user.name zhangsan
git config user.email zhangsan@163.com

       git config --global user.name lisi
       git config --global user.email lisi@163.com
3)、查看本地库的状态：
git status
4)、把工作区中的文件添加到暂存区：
git add 文件名或者  .
5)、从暂存区撤回文件：
git rm --cached 文件名
6)、把文件提交到本地库：
git commit 文件名 -m 日志
7)、查看git的提交日志：
git log
8)、回退到指定版本：
git reset --hard 版本号

​		git reset --hard HEAD^^ 上两个版本

​	9)、还原文件（磁盘中没有，仓库中有）

​		git checkout 文件

​	删除仓库中的文件 git  rm --cached 文件

​	10)、彻底删除文件

​		git rm 文件

## git 忽略文件配置

.gitignore

【!】文件

【!】文件夹

## 分支管理

1)、查看分支：
git branch -v
2)、创建分支：
git branch 分支名称
3)、切换分支：
git checkout 分支名
4)、合并分支：(合并到主干分支)
首先，切换到主干分支；
然后，执行命令：git merge 被合并分支名
5)、删除分支：
git branch -d 分支名

## 使用Github

1)、注册账号：
2)、在Github上创建一个远程仓库：
3)、把本地库中的工程上传到远程仓库：
1)、给远程仓库的url起别名：
git remote -v
git remote add mygithub https://github.com/yangaofei/init-repository.git

​      2)、把本地库中的工程上传到远程仓库：
​          git push mygithub master

获取一次分支

​          git push mygithub master:本地分支名称

删除远程分支  git push -d mygithub  分支

##   从远程仓库中克隆工程到本地库

​      git clone 【-d 指定远程仓库分支】 连接 【自定义名字】

5)、从远程仓库更新文件：
第一种方式：分两步
第一步：从远程仓库抓取数据
git fetch origin master
第二步：合并数据
git merge origin/master
第二种方式：一步拉取
git pull origin 远程分支名称:本地分支名称

## 撤销提交log日志 CRM-project

*# 撤销本地最后一条记录，如果需要撤销最后提交的N条记录，将“1”替换为一个具体的数字“N”即可。* git reset --hard HEAD~1

*# 显示每次修改的文件列表及修改状态* git log --name-status

*# 显示每次修改的文件列表* git log --name-only

*# 显示每次修改的文件列表及文件修改的统计* git log --stat

*# 显示每次修改的文件列表* git whatchanged

*# 显示每次修改的文件列表及统计信息* git whatchanged --stat

*# 显示最后一次的文件改变的具体内容* git show

## idea集成git

1)、idea集成git：File--->Settings-->Version Control--->Git
2)、设置全局签名：
3)、把idea的工程转化为本地库：
4)、把idea工程添加到暂存区
5)、把idea工程提交到本地库
6)、把idea工程分享到远程库
7)、把idea本地更新推送到远程库
8)、从远程库克隆工程到idea
9)、从远程库更新数据到idea工程

## gitlab系统查看及设置等相关命令

sudo gitlab-ctl start # 启动所有 gitlab 组件；

sudo gitlab-ctl stop # 停止所有 gitlab 组件；

sudo gitlab-ctl restart # 重启所有 gitlab 组件；

sudo gitlab-ctl status # 查看服务状态；

sudo gitlab-ctl reconfigure # 启动服务；

sudo vim /etc/gitlab/gitlab.rb # 修改默认的配置文件；

sudo gitlab-ctl tail # 查看日志；