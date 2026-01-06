---
tags:
  - git
---
# 一.Git工作流程
下图展示了 Git 的工作流程：

![](http://www.runoob.com/wp-content/uploads/2015/02/git-process.png)

### 1、克隆仓库

如果你要参与一个已有的项目，首先需要将远程仓库克隆到本地：

```
git clone https://github.com/username/repo.git
cd repo

```

### 2、创建新分支

为了避免直接在 main 或 master 分支上进行开发，通常会创建一个新的分支：

```
git checkout -b new-feature

```

### 3、工作目录

在工作目录中进行代码编辑、添加新文件或删除不需要的文件。

### 4、暂存文件

将修改过的文件添加到暂存区，以便进行下一步的提交操作：

```
git add filename
# 或者添加所有修改的文件
git add .

```

### 5、提交更改

将暂存区的更改提交到本地仓库，并添加提交信息：

```
git commit -m "Add new feature"

```

### 6、拉取最新更改

在推送本地更改之前，最好从远程仓库拉取最新的更改，以避免冲突：

```
git pull origin main
# 或者如果在新的分支上工作
git pull origin new-feature

```

### 7、推送更改

将本地的提交推送到远程仓库：

```
git push origin new-feature

```

### 8、创建 Pull Request（PR）

在 GitHub 或其他托管平台上创建 Pull Request，邀请团队成员进行代码审查。PR 合并后，你的更改就会合并到主分支。

### 9、合并更改

在 PR 审核通过并合并后，可以将远程仓库的主分支合并到本地分支：

```
git checkout main
git pull origin main
git merge new-feature

```

### 10、删除分支

如果不再需要新功能分支，可以将其删除：

```
git branch -d new-feature

```

或者从远程仓库删除分支：

```
git push origin --delete new-feature

```
# 二.Git工作区，暂存区，版本区
- 工作区：在电脑中能看到的目录；
- 暂存区：一般存放在.git目录下的index文件中，故又称索引；
- 版本区：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库
- 三者的关系![[Pasted image 20251212153719.jpg]]
- 图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
- 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
- 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
- **当对工作区修改（或新增）的文件执行 git add 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一新的对象中，而该对象的ID被记录在暂存区的文件索引中。**
- **当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。**
- 当执行 git reset HEAD 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响
- 当执行 git checkout . 或者 git checkout -- <file\> 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
- 当执行 git checkout HEAD . 或者 git checkout HEAD <file\> 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。
### 1、工作区（Working Directory）
工作区是你在本地计算机上的项目目录，你在这里进行文件的创建、修改和删除操作。工作区包含了当前项目的所有文件和子目录。
**特点：**
*   显示项目的当前状态。
*   文件的修改在工作区中进行，但这些修改还没有被记录到版本控制中。
### 2、暂存区（Staging Area）
暂存区是一个临时存储区域，它包含了即将被提交到版本库中的文件快照，在提交之前，你可以选择性地将工作区中的修改添加到暂存区。
**特点：**
*   暂存区保存了将被包括在下一个提交中的更改。
*   你可以多次使用 `git add` 命令来将文件添加到暂存区，直到你准备好提交所有更改。
**常用命令：**
```
git add filename       # 将单个文件添加到暂存区
git add .              # 将工作区中的所有修改添加到暂存区
git status             # 查看哪些文件在暂存区中
```
### 3、版本库（Repository）
- 版本库包含项目的所有版本历史记录。
  每次提交都会在版本库中创建一个新的快照，这些快照是不可变的，确保了项目的完整历史记录。
**特点：**
*   版本库分为本地版本库和远程版本库。这里主要指本地版本库。
*   本地版本库存储在 `.git` 目录中，它包含了所有提交的对象和引用。
**常用命令：**
```
git commit -m "Commit message"   # 将暂存区的更改提交到本地版本库
git log                          # 查看提交历史
git diff                         # 查看工作区和暂存区之间的差异
git diff --cached                # 查看暂存区和最后一次提交之间的差异
```
### 工作区、暂存区和版本库之间的关系
**1、工作区 -> 暂存区**
使用 git add 命令将工作区中的修改添加到暂存区。
```
git add filename
```
**2、暂存区 -> 版本库**
使用 git commit 命令将暂存区中的修改提交到版本库。
```
git commit -m "Commit message"
```
**3、版本库 -> 远程仓库**
使用 git push 命令将本地版本库的提交推送到远程仓库。
```
git push origin branch-name
```
**4、远程仓库 -> 本地版本库**
使用 git pull 或 git fetch 命令从远程仓库获取更新。
```
git pull origin branch-name
# 或者
git fetch origin branch-name
git merge origin/branch-name
```
### 实例
假设你在工作目录中修改了 file.txt：
**1、工作区**
修改 file.txt 并保存。
**2、暂存区**
将修改添加到暂存区：
```
git add file.txt
```
**3、版本库**
将暂存区的修改提交到本地版本库：
```
git commit -m "Update file.txt"
```
**4、远程仓库**
将本地提交推送到远程仓库：
```
git push origin main
```
通过理解工作区、暂存区和版本库的作用及其相互关系，你可以更加高效地使用 Git 进行版本控制和协同开发。
# 三.Git创建仓库
### git init
Git 使用 **git init** 命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以 **git init** 是使用 Git 的第一个命令。

在执行完成 **git init** 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变。
### 使用方法
- 进入你想要创建仓库的目录，或者先创建一个新的目录：
```
mkdir my-project
cd my-project
```

- 使用当前目录作为 Git 仓库，我们只需使它初始化。

```
git init
```
- 该命令执行完后会在当前目录生成一个 .git 目录。
- 使用我们指定目录作为 Git 仓库。
```
git init newrepo
```
- 初始化后，会在 newrepo 目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。

- 如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交：

```
$ git add *.c
$ git add README
$ git commit -m '初始化项目版本'
```

- 以上命令将目录下以 .c 结尾及 README 文件提交到仓库中。

>**注：** 在 Linux 系统中，commit 信息使用单引号 `'`，Windows 系统，commit 信息使用双引号 `"`。

所以在 git bash 中 `git commit -m '提交说明'` 这样是可以的，在 Windows 命令行中就要使用双引号 `git commit -m "提交说明"`。

## git clone

我们使用 **git clone** 从现有 Git 仓库中拷贝项目（类似 **svn checkout**）。

克隆仓库的命令格式为：

```
git clone <repo>
```

如果我们需要克隆到指定的目录，可以使用以下命令格式：

```
git clone <repo> <directory>
```

**参数说明：**

*   **repo:**Git 仓库。
*   **directory:** 本地目录。

比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：

```
$ git clone git://github.com/schacon/grit.git

```

执行该命令后，会在当前目录下创建一个名为 grit 的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录。

```
$ git clone git://github.com/schacon/grit.git mygrit

```

## 配置

git 的设置使用 `git config` 命令。

```
$ git config --list
credential.helper=osxkeychain
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
core.ignorecase=true
core.precomposeunicode=true

```

```
$ git config -e    # 针对当前仓库 

```

或者：

```
$ git config -e --global   # 针对系统上所有仓库

```

设置提交代码时的用户信息：

```
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com

```

如果去掉 **--global** 参数只对当前仓库有效。
# 四.Git基本操作
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
# 五.Git分支管理
### 创建分支

创建新分支并切换到该分支：

```
git checkout -b <branchname>

```

例如：

```
git checkout -b feature-xyz

```

切换分支命令:

```
git checkout (branchname)

```

例如：

```
git checkout main

```

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

### 查看分支

查看所有分支：

```
git branch

```

查看远程分支：

```
git branch -r

```

查看所有本地和远程分支：

```
git branch -a

```

### 合并分支

将其他分支合并到当前分支：

```
git merge <branchname>

```

例如，切换到 main 分支并合并 feature-xyz 分支：

```
git checkout main
git merge feature-xyz

```

### 解决合并冲突

当合并过程中出现冲突时，Git 会标记冲突文件，你需要手动解决冲突。

打开冲突文件，按照标记解决冲突。

标记冲突解决完成：

```
git add <conflict-file>

```

提交合并结果：

```
git commit

```

### 删除分支

删除本地分支：

```
git branch -d <branchname>

```

强制删除未合并的分支：

```
git branch -D <branchname>

```

删除远程分支：

```
git push origin --delete <branchname>

```

## 实例

开始前我们先创建一个测试目录：

```
$ mkdir gitdemo
$ cd gitdemo/
$ git init
Initialized empty Git repository...
$ touch README
$ git add README
$ git commit -m '第一次版本提交'
[master (root-commit) 3b58100] 第一次版本提交
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README

```

### Git 分支管理

#### 列出分支

列出分支基本命令：

```
git branch

```

没有参数时，**git branch** 会列出你在本地的分支。

```
$ git branch
* master

```

此例的意思就是，我们有一个叫做 **master** 的分支，并且该分支是当前分支。

当你执行 **git init** 的时候，默认情况下 Git 就会为你创建 **master** 分支。

如果我们要手动创建一个分支。执行 `git branch (branchname)` 即可。

```
$ git branch testing
$ git branch
* master
  testing

```

现在我们可以看到，有了一个新分支 **testing**。

当你以此方式在上次提交更新之后创建了新分支，如果后来又有更新提交， 然后又切换到了 **testing** 分支，Git 将还原你的工作目录到你创建分支时候的样子。

接下来我们将演示如何切换分支，我们用 git checkout (branch) 切换到我们要修改的分支。

```
$ ls
README
$ echo 'runoob.com' > test.txt
$ git add .
$ git commit -m 'add test.txt'
[master 3e92c19] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
$ ls
README        test.txt
$ git checkout testing
Switched to branch 'testing'
$ ls
README

```

当我们切换到 **testing** 分支的时候，我们添加的新文件 test.txt 被移除了。切换回 **master** 分支的时候，它们又重新出现了。

```
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt

```

我们也可以使用 git checkout -b (branchname) 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。

```
$ git checkout -b newtest
Switched to a new branch 'newtest'
$ git rm test.txt 
rm 'test.txt'
$ ls
README
$ touch runoob.php
$ git add .
$ git commit -am 'removed test.txt、add runoob.php'
[newtest c1501a2] removed test.txt、add runoob.php
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php
$ git checkout master
Switched to branch 'master'
$ ls
README        test.txt


```

如你所见，我们创建了一个分支，在该分支上移除了一些文件 test.txt，并添加了 runoob.php 文件，然后切换回我们的主分支，删除的 test.txt 文件又回来了，且新增加的 runoob.php 不存在主分支中。

使用分支将工作切分开来，从而让我们能够在不同开发环境中做事，并来回切换。

#### 删除分支

删除分支命令：

```
git branch -d (branchname)

```

例如我们要删除 testing 分支：

```
$ git branch
* master
  testing
$ git branch -d testing
Deleted branch testing (was 85fc7e7).
$ git branch
* master

```

#### 分支合并

一旦某分支有了独立内容，你终究会希望将它合并回到你的主分支。 你可以使用以下命令将任何分支合并到当前分支中去：

```
git merge

```

```
$ git branch
* master
  newtest
$ ls
README        test.txt
$ git merge newtest
Updating 3e92c19..c1501a2
Fast-forward
 runoob.php | 0
 test.txt   | 1 -
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php


```

以上实例中我们将 newtest 分支合并到主分支去，test.txt 文件被删除。

合并完后就可以删除分支:

```
$ git branch -d newtest
Deleted branch newtest (was c1501a2).

```

删除后， 就只剩下 master 分支了：

```
$ git branch
* master

```

#### 合并冲突

合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。

```
$ git branch
* master
$ cat runoob.php

```

首先，我们创建一个叫做 change_site 的分支，切换过去，我们将 runoob.php 内容改为:

```
<?php
echo 'runoob';
?>

```

创建 change_site 分支：

```
$ git checkout -b change_site
Switched to a new branch 'change_site'
$ vim runoob.php
$ head -3 runoob.php
<?php
echo 'runoob';
?>
$ git commit -am 'changed the runoob.php'
[change_site 7774248] changed the runoob.php
 1 file changed, 3 insertions(+)
 

```

将修改的内容提交到 change_site 分支中。 现在，假如切换回 master 分支我们可以看内容恢复到我们修改前的 (空文件，没有代码)，我们再次修改 runoob.php 文件。

```
$ git checkout master
Switched to branch 'master'
$ cat runoob.php
$ vim runoob.php    # 修改内容如下
$ cat runoob.php
<?php
echo 1;
?>
$ git diff
diff --git a/runoob.php b/runoob.php
index e69de29..ac60739 100644
--- a/runoob.php
+++ b/runoob.php
@@ -0,0 +1,3 @@
+<?php
+echo 1;
+?>
$ git commit -am '修改代码'
[master c68142b] 修改代码
 1 file changed, 3 insertions(+)

```

现在这些改变已经记录到我的 "master" 分支了。接下来我们将 "change_site" 分支合并过来。

```
$ git merge change_site
Auto-merging runoob.php
CONFLICT (content): Merge conflict in runoob.php
Automatic merge failed; fix conflicts and then commit the result.

$ cat runoob.php     # 打开文件，看到冲突内容
<?php
<<<<<<< HEAD
echo 1;
=======
echo 'runoob';
>>>>>>> change_site
?>

```

我们将前一个分支合并到 master 分支，一个合并冲突就出现了，接下来我们需要手动去修改它。

```
$ vim runoob.php 
$ cat runoob.php
<?php
echo 1;
echo 'runoob';
?>
$ git diff
diff --cc runoob.php
index ac60739,b63d7d7..0000000
--- a/runoob.php
+++ b/runoob.php
@@@ -1,3 -1,3 +1,4 @@@
  <?php
 +echo 1;
+ echo 'runoob';
  ?>

```

在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决

```
$ git status -s
UU runoob.php
$ git add runoob.php
$ git status -s
M  runoob.php
$ git commit
[master 88afe0e] Merge branch 'change_site'

```

现在我们成功解决了合并中的冲突，并提交了结果。

## 命令手册

<table><thead><tr><th><strong>命令</strong></th><th><strong>说明</strong></th><th><strong>用法示例</strong></th></tr></thead><tbody><tr><td><code>git branch</code></td><td>列出、创建或删除分支。它不切换分支，只是用于管理分支的存在。</td><td><code>git branch</code>：列出所有分支<br><code>git branch new-branch</code>：创建新分支<br><code>git branch -d old-branch</code>：删除分支</td></tr><tr><td><code>git checkout</code></td><td>切换到指定的分支或恢复工作目录中的文件。也可以用来检出特定的提交。</td><td><code>git checkout branch-name</code>：切换分支<br><code>git checkout file.txt</code>：恢复文件到工作区<br><code>git checkout &lt;commit-hash&gt;</code>：检出特定提交</td></tr><tr><td><code>git switch</code></td><td>专门用于切换分支，相比 <code>git checkout</code> 更加简洁和直观，主要用于分支操作。</td><td><code>git switch branch-name</code>：切换到指定分支<br><code>git switch -c new-branch</code>：创建并切换到新分支</td></tr><tr><td><code>git merge</code></td><td>合并指定分支的更改到当前分支。</td><td><code>git merge branch-name</code>：将指定分支的更改合并到当前分支</td></tr><tr><td><code>git mergetool</code></td><td>启动合并工具，以解决合并冲突。</td><td><code>git mergetool</code>：使用默认合并工具解决冲突<br><code>git mergetool --tool=&lt;tool-name&gt;</code>：指定合并工具</td></tr><tr><td><code>git log</code></td><td>显示提交历史记录。</td><td><code>git log</code>：显示提交历史<br><code>git log --oneline</code>：以简洁模式显示提交历史</td></tr><tr><td><code>git stash</code></td><td>保存当前工作目录中的未提交更改，并将其恢复到干净的工作区。</td><td><code>git stash</code>：保存当前更改<br><code>git stash pop</code>：恢复最近保存的更改<br><code>git stash list</code>：列出所有保存的更改</td></tr><tr><td><code>git tag</code></td><td>创建、列出或删除标签。标签用于标记特定的提交。</td><td><code>git tag</code>：列出所有标签<br><code>git tag v1.0</code>：创建一个新标签<br><code>git tag -d v1.0</code>：删除标签</td></tr><tr><td><code>git worktree</code></td><td>允许在一个仓库中检查多个工作区，适用于同时处理多个分支。</td><td><code>git worktree add &lt;path&gt; branch-name</code>：在指定路径添加新的工作区并切换到指定分支<br><code>git worktree remove &lt;path&gt;</code>：删除工作区</td></tr></tbody></table>
# 六.Git查看提交历史
- git提交查看历史有两种命令：
- **git log** -  查看历史提交记录
- **git blame <file\> - 以列表形式查看指定文件的历史修改记录
## git log

在使用 Git 提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，我们可以使用 `git log` 命令查看。

**git log** 命令用于查看 Git 仓库中提交历史记录。

**git log** 显示了从最新提交到最早提交的所有提交信息，包括提交的哈希值、作者、提交日期和提交消息等。

**git log** 命令的基本语法：

```
git log [选项] [分支名/提交哈希]

```

常用的选项包括：

*   `-p`：显示提交的补丁（具体更改内容）。
*   `--oneline`：以简洁的一行格式显示提交信息。
*   `--graph`：以图形化方式显示分支和合并历史。
*   `--decorate`：显示分支和标签指向的提交。
*   `--author=<作者>`：只显示特定作者的提交。
*   `--since=<时间>`：只显示指定时间之后的提交。
*   `--until=<时间>`：只显示指定时间之前的提交。
*   `--grep=<模式>`：只显示包含指定模式的提交消息。
*   `--no-merges`：不显示合并提交。
*   `--stat`：显示简略统计信息，包括修改的文件和行数。
*   `--abbrev-commit`：使用短提交哈希值。
*   `--pretty=<格式>`：使用自定义的提交信息显示格式。

针对我们前一章节的操作，使用 `git log` 命令列出历史提交记录如下：

```
$ git log
commit d5e9fc2c811e0ca2b2d28506ef7dc14171a207d9 (HEAD -> master)
Merge: c68142b 7774248
Author: runoob <test@runoob.com>
Date:   Fri May 3 15:55:58 2019 +0800

    Merge branch 'change_site'

commit c68142b562c260c3071754623b08e2657b4c6d5b
Author: runoob <test@runoob.com>
Date:   Fri May 3 15:52:12 2019 +0800

    修改代码

commit 777424832e714cf65d3be79b50a4717aea51ab69 (change_site)
Author: runoob <test@runoob.com>
Date:   Fri May 3 15:49:26 2019 +0800

    changed the runoob.php

commit c1501a244676ff55e7cccac1ecac0e18cbf6cb00
Author: runoob <test@runoob.com>
Date:   Fri May 3 15:35:32 2019 +0800

```

我们可以用 --oneline 选项来查看历史记录的简洁的版本。

```
$ git log --oneline
$ git log --oneline
d5e9fc2 (HEAD -> master) Merge branch 'change_site'
c68142b 修改代码
7774248 (change_site) changed the runoob.php
c1501a2 removed test.txt、add runoob.php
3e92c19 add test.txt
3b58100 第一次版本提交

```

这告诉我们的是，此项目的开发历史。

我们还可以用 --graph 选项，查看历史中什么时候出现了分支、合并。以下为相同的命令，开启了拓扑图选项：

```
*   d5e9fc2 (HEAD -> master) Merge branch 'change_site'
|\  
| * 7774248 (change_site) changed the runoob.php
* | c68142b 修改代码
|/  
* c1501a2 removed test.txt、add runoob.php
* 3e92c19 add test.txt
* 3b58100 第一次版本提交

```

现在我们可以更清楚明了地看到何时工作分叉、又何时归并。

你也可以用 **--reverse** 参数来逆向显示所有日志。

```
$ git log --reverse --oneline
3b58100 第一次版本提交
3e92c19 add test.txt
c1501a2 removed test.txt、add runoob.php
7774248 (change_site) changed the runoob.php
c68142b 修改代码
d5e9fc2 (HEAD -> master) Merge branch 'change_site'

```

如果只想查找指定用户的提交日志可以使用命令：git log --author , 例如，比方说我们要找 Git 源码中 Linus 提交的部分：

```
$ git log --author=Linus --oneline -5
81b50f3 Move 'builtin-*' into a 'builtin/' subdirectory
3bb7256 make "index-pack" a built-in
377d027 make "git pack-redundant" a built-in
b532581 make "git unpack-file" a built-in
112dd51 make "mktag" a built-in

```

如果你要指定日期，可以执行几个选项：--since 和 --before，但是你也可以用 --until 和 --after。

例如，如果我要看 Git 项目中三周前且在四月十八日之后的所有提交，我可以执行这个（我还用了 --no-merges 选项以隐藏合并提交）：

```
$ git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges
5469e2d Git 1.7.1-rc2
d43427d Documentation/remote-helpers: Fix typos and improve language
272a36b Fixup: Second argument may be any arbitrary string
b6c8d2d Documentation/remote-helpers: Add invocation section
5ce4f4e Documentation/urls: Rewrite to accomodate transport::address
00b84e9 Documentation/remote-helpers: Rewrite description
03aa87e Documentation: Describe other situations where -z affects git diff
77bc694 rebase-interactive: silence warning when no commits rewritten
636db2c t3301: add tests to use --format="%N"

```

### 常用选项

限制显示的提交数:

```
git log -n <number>

```

例如，显示最近的 5 次提交：

```
git log -n 5

```

显示自指定日期之后的提交：

```
git log --since="2024-01-01"

```

显示指定日期之前的提交：

```
git log --until="2024-07-01"

```

只显示某个作者的提交：

```
git log --author="Author Name"

```

更多 **git log** 命令可查看 [http://git-scm.com/docs/git-log](http://git-scm.com/docs/git-log) 或使用 `git log --help` 命令查看帮助信息。

## git blame

**git blame** 命令用于逐行显示指定文件的每一行代码是由谁在什么时候引入或修改的。

**git blame** 可以追踪文件中每一行的变更历史，包括作者、提交哈希、提交日期和提交消息等信息。

如果要查看指定文件的修改记录可以使用 git blame 命令，格式如下：

```
git blame [选项] <文件路径>

```

常用的选项包括：

*   `-L <起始行号>,<结束行号>`：只显示指定行号范围内的代码注释。
*   `-C`：对于重命名或拷贝的代码行，也进行代码行溯源。
*   `-M`：对于移动的代码行，也进行代码行溯源。
*   `-C -C` 或 `-M -M`：对于较多改动的代码行，进行更进一步的溯源。
*   `--show-stats`：显示包含每个作者的行数统计信息。

显示文件每一行的代码注释和相关信息：

```
git blame <文件路径>

```

只显示指定行号范围内的代码注释：

```
git blame -L <起始行号>,<结束行号> <文件路径>

```

对于重命名或拷贝的代码行进行溯源：

```
git blame -C <文件路径>

```

对于移动的代码行进行溯源：

```
git blame -M <文件路径>

```

显示行数统计信息：

```
git blame --show-stats <文件路径>

```

git blame 命令是以列表形式显示修改记录，如下实例：

```
$ git blame README 
^d2097aa (tianqixin 2020-08-25 14:59:25 +0800 1) # Runoob Git 测试
db9315b0 (runoob    2020-08-25 16:00:23 +0800 2) # 菜鸟教程 

```

更多内容可以使用 `git blame --help` 查看完整的帮助文档，了解更多选项和使用方式。

## 恢复和回退

Git 提供了多种方式来恢复和回退到之前的版本，不同的命令适用于不同的场景和需求。

以下是几种常见的方法：

*   **`git checkout`**：切换分支或恢复文件到指定提交。
*   **`git reset`**：重置当前分支到指定提交（软重置、混合重置、硬重置）。
*   **`git revert`**：创建一个新的提交以撤销指定提交，不改变提交历史。
*   **`git reflog`**：查看历史操作记录，找回丢失的提交。

### 1、git checkout：检查出特定版本的文件

git checkout 命令用于切换分支或恢复工作目录中的文件到指定的提交。

恢复工作目录中的文件到某个提交：

```
git checkout <commit> -- <filename>

```

例如，将 file.txt 恢复到 abc123 提交时的版本：

```
git checkout abc123 -- file.txt

```

切换到特定提交：

```
git checkout <commit>

```

例如：

```
git checkout abc123

```

这种方式切换到特定的提交时，处于分离头指针（detached HEAD）状态。

### 2、git reset：重置当前分支到特定提交

git reset 命令可以更改当前分支的提交历史，它有三种主要模式：--soft、--mixed 和 --hard。

`--soft`：只重置 HEAD 到指定的提交，暂存区和工作目录保持不变。

```
git reset --soft <commit>

```

`--mixed（默认）`：重置 HEAD 到指定的提交，暂存区重置，但工作目录保持不变。

```
git reset --mixed <commit>

```

`--hard`：重置 HEAD 到指定的提交，暂存区和工作目录都重置。

```
git reset --hard <commit>

```

例如，将当前分支重置到 abc123 提交：

```
git reset --hard abc123

```

### 3、git revert：撤销某次提交

git revert 命令创建一个新的提交，用来撤销指定的提交，它不会改变提交历史，适用于已经推送到远程仓库的提交。

```
git revert <commit>

```

例如，撤销 abc123 提交：

```
git revert abc123

```

### 4、git reflog：查看历史操作记录

git reflog 命令记录了所有 HEAD 的移动。即使提交被删除或重置，也可以通过 reflog 找回。

```
git reflog

```

利用 reflog 可以找到之前的提交哈希，从而恢复到特定状态。例如：

```
git reset --hard HEAD@{3}

```

### 实例

以下是一个综合示例，演示如何使用这些命令恢复历史版本：

查看提交历史：

```
git log --oneline

```

假设输出如下：

```
abc1234 Commit 1
def5678 Commit 2
ghi9012 Commit 3

```

切换到 Commit 2（处于分离头指针状态）：

```
git checkout def5678

```

重置到 Commit 2，保留更改到暂存区：

```
git reset --soft def5678

```

重置到 Commit 2，取消暂存区更改：

```
git reset --mixed def5678

```

重置到 Commit 2，丢弃所有更改：

```
git reset --hard def5678

```

撤销 Commit 2：

```
git revert def5678

```

查看 reflog 找回丢失的提交：

```
git reflog

```

找到之前的提交哈希并恢复：

```
git reset --hard HEAD@{3}

```
# 七.Git标签
如果你达到一个重要的阶段，并希望永远记住提交的快照，你可以使用 `git tag` 给它打上标签。

Git 标签（Tag）用于给仓库中的特定提交点加上标记，通常用于发布版本（如 v1.0, v2.0）。

比如说，我们想为我们的 runoob 项目发布一个 "1.0" 版本，我们可以用 `git tag -a v1.0` 命令给最新一次提交打上（HEAD） "v1.0" 的标签。

**`-a` 选项意为** "创建一个带注解的标签"，不用 `-a` 选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解，我们推荐一直创建带注解的标签。

**标签语法格式：**

```
git tag <tagname>
```

例如：

```
git tag v1.0
```

`-a` 选项可以添加注解：

```
$ git tag -a v1.0 
```

当你执行 git tag -a 命令时，Git 会打开你的编辑器，让你写一句标签注解，就像你给提交写注解一样。

现在，注意当我们执行 `git log --decorate` 时，我们可以看到我们的标签了：

```
*   d5e9fc2 (HEAD -> master) Merge branch 'change_site'
|\  
| * 7774248 (change_site) changed the runoob.php
* | c68142b 修改代码
|/  
* c1501a2 removed test.txt、add runoob.php
* 3e92c19 add test.txt
* 3b58100 第一次版本提交

```

如果我们忘了给某个提交打标签，又将它发布了，我们可以给它追加标签。

例如，假设我们发布了提交 85fc7e7(上面实例最后一行)，但是那时候忘了给它打标签。 我们现在也可以：

```
$ git tag -a v0.9 85fc7e7
$ git log --oneline --decorate --graph
*   d5e9fc2 (HEAD -> master) Merge branch 'change_site'
|\  
| * 7774248 (change_site) changed the runoob.php
* | c68142b 修改代码
|/  
* c1501a2 removed test.txt、add runoob.php
* 3e92c19 add test.txt
* 3b58100 (tag: v0.9) 第一次版本提交

```

如果我们要查看所有标签可以使用以下命令：

```
$ git tag
v0.9
v1.0

```

### 推送标签到远程仓库

默认情况下，git push 不会推送标签，你需要显式地推送标签。

```
git push origin <tagname>

```

推送所有标签：

```
git push origin --tags

```

### 删除轻量标签

本地删除：

```
git tag -d <tagname>

```

远程删除：

```
git push origin --delete <tagname>

```

### 附注标签

附注标签存储了创建者的名字、电子邮件、日期，并且可以包含标签信息。附注标签更为正式，适用于需要额外元数据的场景。

创建附注标签语法：

```
git tag -a <tagname> -m "message"

```

```
git tag -a <tagname> -m "runoob.com标签"

```

PGP 签名标签命令：

```
git tag -s <tagname> -m "runoob.com标签"

```

查看标签信息：

```
git show <tagname>

```

### 实例

以下是一个综合示例，演示如何创建、查看、推送和删除标签。

创建轻量标签和附注标签：

```
git tag v1.0
git tag -a v1.1 -m "runoob.com标签"

```

查看标签和标签信息：

```
git tag
git show v1.1

```

推送标签到远程仓库：

```
git push origin v1.0
git push origin v1.1
git push origin --tags  # 推送所有标签

```

### 删除标签

本地删除：

```
git tag -d v1.0

```

远程删除：

```
git push origin --delete v1.0
```
# 八.Git Flow分支模型
**`master` 分支**：

*   永远保持稳定和可发布的状态。
*   每次发布一个新的版本时，都会从 `develop` 分支合并到 `master` 分支。

**`develop` 分支**：

*   用于集成所有的开发分支。
*   代表了最新的开发进度。
*   功能分支、发布分支和修复分支都从这里分支出去，最终合并回这里。

**`feature` 分支**：

*   用于开发新功能。
*   从 `develop` 分支创建，开发完成后合并回 `develop` 分支。
*   命名规范：`feature/feature-name`。

**`release` 分支**：

*   用于准备新版本的发布。
*   从 `develop` 分支创建，进行最后的测试和修复，然后合并回 `develop` 和 `master` 分支，并打上版本标签。
*   命名规范：`release/release-name`。

**`hotfix` 分支**：

*   用于修复紧急问题。
*   从 `master` 分支创建，修复完成后合并回 `master` 和 `develop` 分支，并打上版本标签。
*   命名规范：`hotfix/hotfix-name`。

![](https://www.runoob.com/wp-content/uploads/2024/07/git-flow.png)

### 分支操作原理

*   Master 分支上的每个 Commit 应打上 Tag，Develop 分支基于 Master 创建。
*   Feature 分支完成后合并回 Develop 分支，并通常删除该分支。
*   Release 分支基于 Develop 创建，用于测试和修复 Bug，发布后合并回 Master 和 Develop，并打 Tag 标记版本号。
*   Hotfix 分支基于 Master 创建，完成后合并回 Master 和 Develop，并打 Tag 1。