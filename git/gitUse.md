﻿
# git的使用手册

## 前言

![git命令图解](https://img-blog.csdnimg.cn/20190729183631588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1NjAxNDcx,size_16,color_FFFFFF,t_70)
注：

* 看懂这篇凌乱的文章就会了，不是看完哦。
* 根据个人理解画的图用于辅助理解，脑回路有些清奇，如有错误请指正。
* 如果可以看懂上面的图，而且命令都能够理解并使用。这篇文章的帮助应该不大。
* 这里只是展示了不同区域之间代码传递的时候使用的命令，使用的时候要注意具体参数。
* 所谓的不同区域大多是一个个的指针而已。
* 修改代码什么的均发生在工作区。

---

___正式开始___

---

## git初始化

```cmd
git init
```

## 添加文件

相当于将工作区文件修改提交到暂存区

```cmd
git add <filename>
```

## 提交修改到版本库

将暂存区的文件修改提交到本地版本库

```cmd
git commit -m ""
```

追加提交

```cmd
git commit -m " this is test amend commit"
git add < filename>
git commit --amend  // 此命令会打开编辑窗口，用于编辑前一次的commit message。
```

上面的三个命令只会产生一个提交记录

## 查看文件之间的区别内容

```cmd
// 工作区
git diff <filename>  工作区暂存区的区别
git diff <commid> <filename> 指定版本和工作区的区别。
// 暂存区
git diff --cached <filename>  工作区和版本库最新版本的区别
git diff --cached <commid> <filename> 指定版本和暂存区的区别。
// 版本之间
git diff <commid> <commitd>  查看两个版本之间的区别
```

## 查看日志

### 显示日志

```cmd
git log  // 全量展示
git log --pretty=oneline  // 将每条记录选取关键信息合成一行展示。
git log --abbrev-commit   // 将commit id 显示为缩写。
git log --graph       // ASCII图展示每次的提交所在分支以及分支分化衍合情况
git log -5        // 展示最近的五条数据提交记录
```

### 过滤日志

```cmd
git log --author <authorName>  按作者名过滤
git log --grep <commit message> 按提交信息
git log -- <fileName>  按文件名
```

> 以上的这参数可以组合使用

## 引用日志

&emsp;&emsp;git reflog 查看所有版本操作相关的，例如版本回退等等的记录，HEAD指针指向的位置的记录，即ref的log，即对版本库中的代码的增删改查记录。

>reflog 是存储于本地的日志并不会共享。

## 版本回退

在git 中HEAD表示当前的版本，HEAD^ 表示上一个版本，HEAD^^ 表示上上个版本。较多的试试直接用数字指定。例如：HEAD~10。

> 注意：^ 字符是windows的cmd.exe 中的元字符。windows下使用HEAD^可能会报错  
> fatal: ambiguous argument 'HEAD  
> ': unknown revision or path not in the working tree.  
> 这时要使用HEAD"^"或者"HEAD^"，来避免它。

例如

```cmd
git reset --hard HEAD^ 强制回退上一版本
git reset --hard HEAD~1
git reset --hard < commit id> 默认为HEAD
```

---

***以下操作慎用，命令使用之后不一定能够有反悔的机会。因为只有在对版本库中的文件进行增删改查的时候，在git log或者git reflog中才会有日志记录。尤其在使用--hard 的时候更有可能会直接无提示的清空掉工作区和暂存区的数据***

---

* 从工作区撤销修改  
将该文件在工作区的修改全部撤销，相当于将工作区的文件修改为和暂存区的一致。

  ```git
  git checkout -- < filename>
  ```

* 从暂存区撤销修改  
如果文件已经提交到了暂存区，首先需要撤销暂存区的修改，

  ```cmd
  git reset HEAD < filename>
  ```

  将暂存区的修改回退到工作区，然后根据需要决定是否撤销工作区的修改，如果需要继续撤销，参考上一条。

* 从本地版本库撤销修改

  ```cmd
  git reset <options> <commit id>
  // 回退 options 默认为mixed commit id默认为HEAD

  git reset --mixed 保留工作区，把暂存区的改变回退到工作区，移动HEAD指针，暂存区和HEAD指针的版本一致。
  git reset --soft  保留工作区和暂存区，仅移动HEAD指针，所有的reset都会执行这个操作。
  git reset --hard  清空工作区和暂存区的改变。移动HEAD指针，暂存区和工作区区和HEAD指针的版本一致。
  git reset --merge 如果暂存区为空，此命令无效果。如果工作区和暂存区存在对同一文件的修改则会失败。一个文件工作区无修改，暂存区存在修改的时候生效，即工作区和暂存区相同但是还没有提交到版本库。此命令会清空工作区和暂存区。移动HEAD指针。这类文件的暂存区和工作区同步为HEAD指针的版本一致。  
  git reset --keep 保留工作区，把暂存区的改变也回退到工作区，移动HEAD指针，暂存区和HEAD指针的版本一致。

  ```

  > --mixed 和 --keep的区别在于，恢复到历史版本的时候，如果工作区或者暂存区存在修改的文件在要恢复历史版本和HEAD指向的版本之间存在不同，mixed会直接恢复，--keep会失败。

* 撤销远程库修改

  先撤销本地的修改，然后推送到远程库即可。

  ```cmd
  git push -f // 强制推送到远程库  慎用
  ```

* 只恢复指定版本的内容其他版本不变。可以使用revert（反做）命令。

  ```cmd
  git revert <commit ID>
  ```

  revert 命令用于反做某一次的提交，然后形成一条新的提交记录。

* 只恢复指定的文件其他文件不变

  ```cmd
  git checkout <commit id> fileName
  ```

> git reset、git revert、git checkout  
> git reset   指的是恢复到某一个版本。这个版本之后的commit都会被放弃。  
> git revert  指得的是恢复某一个版本的修改。不影响其它的commit。反做这个版本的更改，并形成一条新的commit  
> git checkout 是指恢复某一个文件，并不移动HEAD指针，相当于只是读取某个文件的历史版本覆盖到工作区和缓存区。

---

> 注意： 工作区和暂存区被清空的时候可能会没有提示  **慎用**

## 从版本库中删除文件

```cmd
git rm < filename>
```

## 远程库

### 添加远程库

添加远程库并将远程库命名为 origin ,命名可以自定义。但是一般都是origin吧。

```cmd
git remote add origin git@github.com:************.git // 添加远程库
```

### 推送

将master 中的修改推送到远程库

```cmd
git push -u origin master
git push origin <commit id>:master // 只推送指定的commit
```

-u 表示把本地的master 分支和远程的master关联起来，第一次推送的时候使用，之后就可以只使用git push 命令进行推送了。

git push -f origin master 强制覆盖远程库，，这个一般应该用不着

### 拉取

将远程库拉取到本地

```cmd
git pull
// 下面是拉取到本地后强制覆盖掉本地的修改
git fetch  // 相当于拉取到本地存储的远程库的副本中
git rebase // 从本地的远程库副本中恢复到本地库。
git reset --hard origin/master // 从远程库副本中强制恢复，覆盖本地库
```

### 克隆远程的仓库

```cmd
git clone git@github.com:******.git  ## 默认克隆master 分支
git clone -b < branchName> git@github.com:******.git 克隆指定的分支
```

git clone 之后
使用git branch -r 查看远程仓库所有的分支，
git checkout < remote branchName> 便可以获取指定的远程仓库的分支了。

### 查看远程库信息

git remote
git remote -v 查看远程库信息的信息

```cmd
F:\git\gitskills>git remote -v
origin  git@github.com:****.git (fetch)  拉取分支地址
origin  git@github.com:****.git (push) 推送分支地址
```

## 分支操作

### 创建分支

 创建dev分支并切换到dev分支。

```cmd
git checkout -b dev
```

相当于下面的两个命令的集合。

```cmd
git branch dev       创建dev分支
git checkout dev     切换到dev
```

重命名当前分支

```cmd
git branch -m <new Name>
```

### 查看分支

git branch 查看本地的分支信息
git branch --remote 查看远程的分支信息 简写 git branch -r
git branch --all 查看所有的分支信息本地+远程 简写：-a

### 合并分支

git merge dev   将dev 分支合并到当前分支，无冲突的时候，fast-forward模式。  
git merge --no-ff -m "" dev  将dev 分支合并到当前分支，使用普通模式。不使用fast-forward模式。如果没有-m参数会自动生成一条commit message。  
普通模式合并可以从log中看到做过合并。  
fast-forward看不出做过合并。

git cherry-pick hash-code 合并指定的提交记录

#### 合并冲突

所谓解决冲突就是将冲突的文件，编辑为我们需要的内容然后提交。

### 删除分支

删除dev分支

```cmd
git branch -d dev
```

存在没有合并内容的分支，会提示不允许删除，如果确定需要强制删除该分支，使用。

```cmd
git branch -D dev
```

### 推送到远程分支

把该分支上的修改提交到远程库 即本地master 推送到origin的master

```cmd
git push origin master
```

>关于分支  
master 分支需要时刻和远程的一致  
dev 分支也需要和远程的一致  
bug 分支一般不需要  
feature 分支取决于是否有人合作，（新功能分支）

### 创建本地分支和远程分支之间的链接

```cmd
git branch --set-upstream-to=origin/dev dev
```

将远程的dev 分支和本地的dev分支连接起来
我们使用git clone *** 远程仓库的时候默认只会克隆master分支。  
&emsp;&emsp;如果我们在本地创建了dev 分支之后，想要向远程库的dev分支提交数据，需要使用这个命令将两个仓库关联起来然后才可以提交代码。

## 贮藏（stash）

为了不和暂存区的理解混淆，我把它直译为贮藏（或者“储藏”也可以）。

### 贮藏工作状态

贮藏工作状态即存储下工作区和暂存区的状态。

```cmd
git stash
// 贮藏并添加说明
git stash save ""
```

### 查看贮藏的相关信息

查看贮藏列表

```cmd
git stash list
```

查看某一个贮藏记录中的工作状态和当前工作状态的区别

```cmd
git stash show stash@{0}
// 查看更具体不同的内容
git stash show stash@{0} -p
```

### 从贮藏中恢复工作状态

```cmd
git stash apply  // 从stash记录中恢复工作区并不删除stash记录
git stash pop   // 恢复工作区的同时删除记录
git stash apply stash@{0} 恢复到指定的记录
git stash apply/pop --index 恢复工作区的同时恢复暂存区
```

### 清除贮藏记录

```cmd
git stash drop  //  删除stash的记录 默认最新，可以指定
git stash clear 清空列表
```

### 从stash 创建branch

git stash branch

## rebase命令

&emsp;&emsp;rebase 命令，目前有以下三个功能。

### 合并多条提交记录

&emsp;&emsp;我们在本地针对同一个文件进行了多次提交，如果直接推送到远程库，提交的之后，查看远程库中commit记录会非常的多。我们可以将这多条的commit 记录合并为一条记录。
例如。我对有三条已经提交的记录。进行合并

```cmd
F:\test\gitskills>git log --pretty=oneline --abbrev-commit -5
a1728f3 (HEAD -> dev) this is thired 333333
a10dea5 this is second 222
572c1e7 this is first
1a9c5cf merge test     // 这是基础版本
9031e2d (test-me) test m e branch
```

```cmd
git rebase -i HEAD~3
```

会有如下输出。中文翻译是我自己写的。

```cmd
pick 572c1e7 this is first
pick a10dea5 this is second 222
pick a1728f3 this is thired 333333

# Rebase 1a9c5cf..a1728f3 onto 1a9c5cf (3 commands)
#
# Commands:
# 使用这一次提交
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# 使用这一次提交，但是把它融入到前一次提交中。
# s, squash <commit> = use commit, but meld into previous commit
# 使用这一次提交，把它融入到前一次提交中，但是抛弃提交记录中的log message
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
```

将前三行修改为

```cmd
p 572c1e7 this is first
s a10dea5 this is second 222
s a1728f3 this is thired 333333
```

&emsp;&emsp;保存文件后，会自动弹出下面的内容，因为我们选取的是第一条记录将其他两条记录合并进来，我们修改下第一行记录的commit message。之所以修改第一样是因为在一行展示log的时候，会看不到换行之后的message。但是实际上message 是合并了的。多行查看log的时候会看到。

```cmd
p 572c1e7 this is first
# This is a combination of 3 commits.
# This is the 1st commit message:

this is first  !!!修改这里的message

# This is the commit message #2:

this is second 222

# This is the commit message #3:

this is thired 333333

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Jul 27 18:55:57 2019 +0800
#
# interactive rebase in progress; onto 1a9c5cf
# Last commands done (3 commands done):
#    squash a10dea5 this is second 222
#    squash a1728f3 this is thired 333333
```

保存成功之后，在查看提交记录。可以看到三条合并记录被合并成了一条。commit message 也会合并。

```cmd
F:\test\gitskills>git log --pretty=oneline --abbrev-commit -3
27c3c39 (HEAD -> dev) this is first message for rebase
1a9c5cf merge test   // 这个是基础版本
9031e2d (test-me) test m e branch
```

### 也可以用于删除commit 记录

☺我是不会说的，自己理解吧。

### 分支合并

git rebase 将本分支的修改排到远程分支的修改之后。提交记录里面就不会分支合并的记录。

&emsp;&emsp;例如A、B、C 三个分支。B，C分支是均从A切出。然后在B分支上进行了commit操作，然后将它合并到了A分支。这个时候A分支就会比C分支的快上一个commit。此时如果我们需要把C分支的commit合并到A分支的话，首先要在C分支把A分支比C分支快的一个commit合并过来，然后再将C分支合并到A分支。  

将A分支比C分支快的一个commit合并过来这个操作， 有两种方法进行：

1. git merge / （git pull  针对远程分支）  
  如果有冲突，处理冲突然后执行git add --> git commit -m "".  
  处理完成后的日志记录。

    ```cmd
    F:\test\gitskills>git log --pretty=oneline --graph --abbrev-commit -5
    *   cd87169 (HEAD -> testRebase) Merge branch 'dev' into testRebase  // 产生的新的commit记录
    |\
    | * 8d7c328 in dev for test rebase
    * | bd601fe commit in testRebase
    |/
    * 27c3c39 this is first message for rebase // 这个是切分支的时候的基础版本。
    ```

1. 使用git rebase合并

    ```cmd
    git rebase dev
    // 如果两个分支之间创建了链接则可以直接执行 git rebase
    ```

    如果需要处理冲突，处理冲突后执行 git add < conflictFileName>   --然后执行->  git rebase --continue，即可继续完成rebase。
    处理完成后的日志记录。和前面的一种方法相比干净了些，不会产生新的提交记录。
  
    ```cmde
      F:\test\gitskills>git log --pretty=oneline --graph --abbrev-commit -5
      2263f26 (HEAD -> testRebase) commit in testRebase
      8d7c328 (dev) in dev for test rebase
      27c3c39 this is first message for rebase  // 这个是切分支的时候的基础版本
    ```

## 标签TAG

标签相当于给某个commit id创建了一个别名

```cmd
git tag V1.0 默认是当前的HEAD
git tag v1.9 < commit ID> 给指定commit 创建别名
git tag 查看所有的tag tag 是按照字母排序的。
git tag -a < tagName> -m "" 创建tag 并添加说明
git show < tagName> 查看该tag 的详细信息

git tag -d < tagName> 删除标签
git push origin < tagName> 推送指定的标签到远程库
git push origin --tags 推送所有的标签到远程库
```

删除远程库的tag  
首先删除本地的tag

```cmd
git tag -d < tagName>
```

然后删除远程库

```cmd
git push origin :refs/tags/< tagName>
```

## 忽略文件

使用 .gitignore

## git配置信息

```cmd
// 禁止 git 自动将LF转换为CRLF
git config --global core.autocrlf false
```

使用配置给git命令添加缩写

```cmd
git config --global alias.st status
// 有用的快捷键
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

--global 全局配置，不加的话只对当前库生效

查看配置

```cmd
git config --global -l
```

## 命令撤销

误操作的时候试试
git XXX --abort

## git报错记录

### Your local changes to the following files would be overwritten by merge

git 执行一些命令的时候出现如下错误

```cmd
error: Your local changes to the following files would be overwritten by merge:
// 下面的内容有的命令有,有的没有,但是并不影响，
Please commit your changes or stash them before you merge.
```

错误原因：你当前执行的命令会覆盖掉工作区或者缓存区的未提交到版本库的修改，导致工作区缓存区的修改丢失，所以git拒绝了你的操作。

解决方法：

1. 使用stash存储下当前的工作区和缓存区，执行完要执行的命令后，再从stash恢复。

1. 提交当前的工作区和缓存区的内容到本地版本库，然后再执行命令。

1. 放弃工作区和缓存区的修改，

  >放弃工作区和缓存区的修改，由于还没有提交到版本库是不会有记录的，恢复可能不大, 慎用!!!
---

方法有：  
  3.1. git reset 具体使用查看下这个命令的介绍。  
  3.2. 执行命令的时候试试 --hard 或者 -f 参数。  

---

### LF will be replaced by CRLF in xxxxxxxxx

```cmd
warning: LF will be replaced by CRLF in xxxxxxxxxx
```

警告原因：本地的文件使用的是LF 作为换行符，windows（其它不晓得）下git的默认配置会有一个 autocrlf，即自动把LF转换为CRLF。

影响：如果有的人本地是LF有的人是CRLF，有的人自动转换有人不转换。最终有的人提交的时候会发现几乎所有的文件都有修改提示，而实际上只是换行符发生了变化而已。

解决方法：修改配置，建议，对同一个库提交代码的所有人应该统一配置。提交的时候使用统一的换行符号。

```cmd
git config –global core.autocrlf false  // 关闭自动转换为crlf
```

---

### fatal: ambiguous argument 'HEAD

```cmd
f:\test\liu\gitskills>git reset HEAD^
More?
More?
fatal: ambiguous argument 'HEAD
': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
```

原因：^ 是windows下cmd.exe的元字符，导致windows对命令的解析出现了问题。

解决方法：

执行命令的时候 使用HEAD"^"、"HEAD^"、HEAD~、commit id等等。

## 上面的都是瞎扯试试

[Github Desktop](https://desktop.github.com/)
