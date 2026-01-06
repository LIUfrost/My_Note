---
date: 2025/12/14
tags:
  - git
---
# 一.GIT建立仓库
### 1.创建新仓库
- `mkdir xxx`先新建一个文件夹
- `cd xxx`进入文件夹
- `git init`进行初始化
>`init`：在当前目录下生成一个.git文件
>`git init xxx/xxx`:在指定目录中生成一个.git文件
### 2.克隆老仓库
- `git clone URL`从给定的URL中克隆原有的仓库
- `git clone URL xxx//xxx`:在给定目录中克隆原有的仓库
### 3.创建云仓库
- 在VScode点`联系到`->`github`->`workspace`
 >这种情况下，不会再本地克隆原有仓库；
> 只是在github的云空间中云端开发
# 二.GIT开发常用命令
### 1.分支命令
- `git branch xxx`:创建xxx分支
- `git checkout xxx`:将head移到xxx上
>xxx包括：分支名称，tag，commit的哈希值......
- `git checkout -b xxx`:创建xxx分支，并将head移到这个新分支上
- `git merge xxx`:将xxx分支上的所有commit融合到head的分支上
- `git rebase xxx`:在xxx下创建新的commit，并将head上的所有commit融合到这上面
>merge和rebase的区别：融合到的位置不同，
>且merge原分支仍存在，rebase原分支消失
![[Pasted image 20251214095833.png|150]]![[Pasted image 20251214100239.png|142]]
- `git branch -d xxx`：删除xxx分支
- `git branch -D xxx`：强行删除xxx分支
>[-d]若该分支未进行融合，若未合并，删除会被阻止；
>[-D]不管有没有融合，直接删除；
- `git branch`：查看本地所有分支
- `git branch -a`：查看本地加云端所有分支
- `git branch -vv`：查看本地和云端分支的对应关系
### 2.head移动
- `git checkout xxx`:将head移到xxx上
- `xxx^`:代表xxx的前一个commit
- `xxx~x`:代表xxx的前面第x个commit
>[如]：git checkout head^^;
>     git checkout head~2;
- **删除指令**
- `git reset xxx`:将head移回xxx上，并将xxx后面的删除
- `git revert xxx`：在xxx后建立一个新的commit，并且此次commit的改动为：抵消xxx上一次commit的操作
### 3.远程操作
#### 上传
**I.add操作**
`git add xxx`:将xxx提交到暂存区
- xxx可以是文件名，代表要将他修改后的样子提交到云端；
`git add .`：将所有的修改提交到暂存区

**II.commit操作**
`git commit -m "xxxxxx"`:提交暂存区中的所有更改，并且这次提交的信息为”xxxxx“

**III.remote操作**
`git remote -v`:检查是否配置了远程仓库，若没有，需要先添加一个
`git remote add origin https://github.com/你的用户名/你的仓库名.git`:在github上创建好仓库后，bash里添加仓库
**III.push操作**
`git push`：将目前分支上的所有更改推送至远程仓库（若远仓中没有与之对应的分支，则不推送）
`git push -u origin xxx`：在远端建立xxx分支，并与同名本地分支关联

