---
url: https://www.runoob.com/git/git-basic-operations.html
title: Git 基本操作
author: runoob.com
aliases: 
 - 
date: 2025-12-12 20:44:00
tags:

banner: "https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg"
banner_icon: 🔖
---
[Git 创建仓库](https://www.runoob.com/git/git-create-repository.html "Git 创建仓库") [Git 分支管理](https://www.runoob.com/git/git-branch.html "Git 分支管理")

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。

本章将对有关创建与提交你的项目快照的命令作介绍。

Git 常用的是以下 6 个命令：**git clone**、**git push**、**git add** 、**git commit**、**git checkout**、**git pull**，后面我们会详细介绍。

![](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)

**说明：**

*   workspace：工作区
*   staging area：暂存区 / 缓存区
*   local repository：版本库或本地仓库
*   remote repository：远程仓库

一个简单的操作步骤：

```
$ git init    
$ git add .    
$ git commit  

```

*   git init - 初始化仓库。
*   git add . - 添加文件到暂存区。
*   git commit - 将暂存区内容添加到仓库中。

### 创建仓库命令

下表列出了 git 创建仓库的命令：

<table><tbody><tr><th>命令</th><th>说明</th></tr><tr><td><code><a href="/git/git-init.html" rel="noopener noreferrer" target="_blank">git init</a></code></td><td>初始化仓库</td></tr><tr><td><code><a href="/git/git-clone.html" rel="noopener noreferrer" target="_blank">git clone</a></code></td><td>拷贝一份远程仓库，也就是下载一个项目。</td></tr></tbody></table>

## 提交与修改

Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

下表列出了有关创建与提交你的项目的快照的命令：

<table><tbody><tr><th>命令</th><th>说明</th></tr><tr><td><code><a href="/git/git-add.html" rel="noopener noreferrer" target="_blank">git add</a></code></td><td>添加文件到暂存区</td></tr><tr><td><code><a href="/git/git-status.html" rel="noopener noreferrer" target="_blank">git status</a></code></td><td>查看仓库当前的状态，显示有变更的文件。</td></tr><tr><td><code><a href="/git/git-diff.html" rel="noopener noreferrer" target="_blank">git diff</a></code></td><td>比较文件的不同，即暂存区和工作区的差异。</td></tr><tr><td><code><a href="/git/git-difftool.html" rel="noopener noreferrer" target="_blank">git difftool</a></code></td><td>使用外部差异工具查看和比较文件的更改。</td></tr><tr><td><code><a href="/git/git-range-diff.html" rel="noopener noreferrer" target="_blank">git range-diff</a></code></td><td>比较两个提交范围之间的差异。</td></tr><tr><td><code><a href="/git/git-commit.html" rel="noopener noreferrer" target="_blank">git commit</a></code></td><td>提交暂存区到本地仓库。</td></tr><tr><td><code><a href="/git/git-reset.html" rel="noopener noreferrer" target="_blank">git reset</a></code></td><td>回退版本。</td></tr><tr><td><code><a href="/git/git-rm.html" rel="noopener noreferrer" target="_blank">git rm</a></code></td><td>将文件从暂存区和工作区中删除。</td></tr><tr><td><code><a href="/git/git-mv.html" rel="noopener noreferrer" target="_blank">git mv</a></code></td><td>移动或重命名工作区文件。</td></tr><tr><td><code><a href="/git/git-notes.html" rel="noopener noreferrer" target="_blank">git notes</a></code></td><td>添加注释。</td></tr><tr><td><code><a href="/git/git-checkout.html" rel="noopener noreferrer" target="_blank">git checkout</a></code></td><td>分支切换。</td></tr><tr><td><code><a href="/git/git-switch.html" rel="noopener noreferrer" target="_blank">git switch </a>（Git 2.23 版本引入）</code></td><td>更清晰地切换分支。</td></tr><tr><td><code><a href="/git/git-restore.html" rel="noopener noreferrer" target="_blank">git restore </a>（Git 2.23 版本引入）</code></td><td>恢复或撤销文件的更改。</td></tr><tr><td><code><a href="/git/git-show.html" rel="noopener noreferrer" target="_blank">git show</a></code></td><td>显示 Git 对象的详细信息。</td></tr></tbody></table>

### 提交日志

<table><tbody><tr><th>命令</th><th>说明</th></tr><tr><td><code><a href="/git/git-commit-history.html#git-log" rel="noopener noreferrer" target="_blank">git log</a></code></td><td>查看历史提交记录</td></tr><tr><td><code><a href="/git/git-commit-history.html#git-blame" rel="noopener noreferrer" target="_blank">git blame &lt;file&gt;</a></code></td><td>以列表形式查看指定文件的历史修改记录</td></tr><tr><td><code><a href="/git/git-shortlog.html" rel="noopener noreferrer" target="_blank">git shortlog</a></code></td><td>生成简洁的提交日志摘要</td></tr><tr><td><code><a href="/git/git-describe.html" rel="noopener noreferrer" target="_blank">git describe</a></code></td><td>生成一个可读的字符串，该字符串基于 Git 的标签系统来描述当前的提交</td></tr></tbody></table>

### 远程操作

<table><tbody><tr><th>命令</th><th>说明</th></tr><tr><td><code><a href="/git/git-remote.html" rel="noopener noreferrer" target="_blank">git remote</a></code></td><td>远程仓库操作</td></tr><tr><td><code><a href="/git/git-fetch.html" rel="noopener noreferrer" target="_blank">git fetch</a></code></td><td>从远程获取代码库</td></tr><tr><td><code><a href="/git/git-pull.html" rel="noopener noreferrer" target="_blank">git pull</a></code></td><td>下载远程代码并合并</td></tr><tr><td><code><a href="/git/git-push.html" rel="noopener noreferrer" target="_blank">git push</a></code></td><td>上传远程代码并合并</td></tr><tr><td><code><a href="/git/git-submodule.html" rel="noopener noreferrer" target="_blank">git submodule</a></code></td><td>管理包含其他 Git 仓库的项目</td></tr></tbody></table>

## Git 文件状态

Git 的文件状态分为三种：工作目录（Working Directory）、暂存区（Staging Area）、本地仓库（Local Repository）。了解这些概念及其交互方式是掌握 Git 的关键。

### 工作目录（Working Directory）

工作目录是你在本地计算机上看到的项目文件。它是你实际操作文件的地方，包括查看、编辑、删除和创建文件。所有对文件的更改首先发生在工作目录中。

在工作目录中的文件可能有以下几种状态：

*   **未跟踪（Untracked）**：新创建的文件，未被 Git 记录。
*   **已修改（Modified）**：已被 Git 跟踪的文件发生了更改，但这些更改还没有被提交到 Git 记录中。

### 暂存区（Staging Area）

暂存区，也称为索引（Index），是一个临时存储区域，用于保存即将提交到本地仓库的更改。你可以选择性地将工作目录中的更改添加到暂存区中，这样你可以一次提交多个文件的更改，而不必提交所有文件的更改。

*   使用 `git add <filename>` 命令将文件从工作目录添加到暂存区。
*   使用 `git add .` 命令将当前目录下的所有更改添加到暂存区。

```
git add <filename>  # 添加指定文件到暂存区
git add .           # 添加所有更改到暂存区

```

### 本地仓库（Local Repository）

本地仓库是一个隐藏在 `.git` 目录中的数据库，用于存储项目的所有提交历史记录。每次你提交更改时，Git 会将暂存区中的内容保存到本地仓库中。

使用 `git commit -m "commit message"` 命令将暂存区中的更改提交到本地仓库。

```
git commit -m "commit message"  # 提交暂存区的更改到本地仓库

```

### 文件状态的转换流程

**未跟踪（Untracked）**： 新创建的文件最初是未跟踪的。它们存在于工作目录中，但没有被 Git 跟踪。

```
touch newfile.txt  # 创建一个新文件
git status         # 查看状态，显示 newfile.txt 未跟踪

```

**已跟踪（Tracked）**： 通过 `git add` 命令将未跟踪的文件添加到暂存区后，文件变为已跟踪状态。

```
git add newfile.txt  # 添加文件到暂存区
git status           # 查看状态，显示 newfile.txt 在暂存区

```

**已修改（Modified）**： 对已跟踪的文件进行更改后，这些更改会显示为已修改状态，但这些更改还未添加到暂存区。

```
echo "Hello, World!" > newfile.txt  # 修改文件
git status                          # 查看状态，显示 newfile.txt 已修改

```

**已暂存（Staged）**： 使用 `git add` 命令将修改过的文件添加到暂存区后，文件进入已暂存状态，等待提交。

```
git add newfile.txt  # 添加文件到暂存区
git status           # 查看状态，显示 newfile.txt 已暂存

```

**已提交（Committed）**： 使用 `git commit` 命令将暂存区的更改提交到本地仓库后，这些更改被记录下来，文件状态返回为已跟踪状态。

```
git commit -m "Added newfile.txt"  # 提交更改
git status                         # 查看状态，工作目录干净

```