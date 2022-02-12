---
title: git
date: 2020-06-17 12:14:31
tags: git
categories: git
---


# 简介

## 什么是git

分布式版本控制系统

<!--more-->

## 集中式 vs 分布式 版本控制系统

集中式版本控制系统版本库存放在中央处理器，需要联网，服务器挂了就都挂了

分布式版本控制系统，没有“中央服务器”，每台电脑都是完整的版本库，不需要联网，安全性高

# 创建

## 创建版本库（repository）

```
mkdir <name for file>
cd <file path>
git init 
```

过`git init`命令把这个目录变成Git可以管理的仓库，生成`.git`目录（隐藏）

## 添加文件到版本库—注意事项

版本控制系统，本质是跟踪文本文件的改动。所以图片片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

Microsoft的Word格式是二进制格式，所以要用纯文本编写文件

万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，会带来很多错误。

```
git add <file>
git commit -m <message>
```

# 版本回退、修改、删除

## 查看

```
git status %查看仓库状态,是否有文件被修改过
```

工作区（**working tree**），暂存区（**index /stage**），本地仓库（**repository**）

```
git diff <file>  %working tree与index差别
git diff --cached <file>  %index与repository差别
git diff HEAD <file>  %查看working tree和repository差别。HEAD代表的是新commit的信息。
```

![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

## 版本回退至最近的commit

### 查看版本号

```
git log %查看提交日志
```

输出信息（eg）

```
$ git log

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)	% 版本号
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL	% <message>

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

```
git log --pretty=oneline
```

输出信息（eg）

```
$ git log --pretty=oneline

1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

* 版本号（commit id）是一个很大的十六进制数。多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了
* `HEAD` 表示当前版本，`HEAD^`上一个版本,`HEAD^^`上上版本，`HEAD~100`上一百个版本

### 回退

```
git reset --hard HEAD^       %回退到上一个版本
```

若想再回到最新版本，需要找到它的版本号。此时使用`git log`看不到最新版本，使用`git reflog`查看每一次命令。

``` git
git reset --hard 1094a      %版本号没必要写全，前几位就可以了，Git会自动去找
```

### 撤销修改

#### 撤销工作区

```
$ git checkout -- <file>  %把在工作区的修改全部撤销
```

即回到最近一次`git commit`或`git add`时的状态

**不要忘记`--`  ， `git checkout <>`是切换到另一个分支**

#### 撤销暂存区

```
git reset HEAD <file> %把暂存区的修改撤销掉（unstage），重新放回工作区
git checkout -- <file>  %把在工作区的修改全部撤销
```

#### 撤销版本库

```
git reset --hard HEAD^       %回退到上一个版本
```

前提：未推送到远程仓库

### 删除文件

```
git rm test.txt
git commit -m "remove test.txt"
```

### 恢复误删文件

```
git checkout -- test.txt
```

注意：从来没有被添加到版本库就被删除的文件，是无法恢复的

# 远程仓库

## 基本

+ 本地Git仓库和GitHub仓库之间的传输是通过SSH加密

+ 用户主目录 -> .ssh目录     找到对应的`id_rsa`和`id_rsa.pub`两个文件

+ `id_rsa`是私钥，不能泄露出去;`id_rsa.pub`是公钥，可以告诉任何人

+ 登陆GitHub，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容

## 本地库添加到远程库（不常用，一般克隆）

1. 创建本地仓库

2. github上创建仓库

3. 关联仓库

   ```
   $ git remote add origin 
   git@github.com:<github_user_name>/<github_repo_name >.git
   ```

   这里是ssh推送，速度快；也可以使用http推送,但是每次需要输入口令。

4. 将本地库master分支推送到远程

   ```
   $ git push -u origin master
   ```

   远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令为

   ```
   $ git push origin master
   ```

## 从远程库克隆

1. github上创建仓库

2. 克隆到本地

   ```
   git clone git@github.com:m<user_name>/<repo_name>.git
   ```

   ```
   git clone https://github.com/<user_name>/<repo_name>.git
   ```

# 分支管理

## 创建与合并分支

### 理解

1. 一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点

![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

2. 当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上

![git-br-create](https://www.liaoxuefeng.com/files/attachments/919022363210080/l)

3. 从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变

![git-br-dev-fd](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

4. 假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

5. 合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支

![git-br-rm](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)

### 实战

1. 创建`dev`分支，然后切换到`dev`分支

   1.1 法一

      ```
      $ git checkout -b dev
      ```

   `it checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令

   ``` 
   $ git branch dev     % 创建
   $ git checkout dev   % 切换
   ```

   + 撤销修改则是`git checkout -- <file>`

   1.2 法二

   切换分支这个动作，用`switch`更科学

   ```
   $ git switch -c dev   %创建并切换
   $ git switch master   %创建
   ```

   ---

   用`git branch`命令查看当前分支

   ```
   $ git branch
   * dev   % 当前分支前面会标一个*号
     master
   ```

2. 在`dev`分支上修改，提交

   ```
   $ git add readme.txt 
   $ git commit -m "branch test"
   ```

3. 切换回master

   ```
   $ git checkout master
   ```

   ![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

4. 把`dev`分支的工作成果合并到`master`分支

   ```
   $ git merge dev     %用于合并指定分支到当前分支
   
   Updating d46f35e..b17d20e
   Fast-forward
    readme.txt | 1 +
    1 file changed, 1 insertion(+)
   ```

   `Fast-forward`告诉我们这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

5. 删除`dev`分支

   ```
   $ git branch -d dev
   ```


## 解决冲突

原因：`master`分支和`feature1`分支各自都分别有新的提交



![git-br-feature1](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

这种情况下，Git无法执行“快速合并”，必须手动解决冲突后再提交

1. 找到冲突文件（2种）

   ```
   $ git merge feature1
   Auto-merging readme.txt
   CONFLICT (content): Merge conflict in readme.txt
   Automatic merge failed; fix conflicts and then commit the result.
   ```

   `git status`也可以告诉我们冲突的文件

   ```
    git status
   ```

2.  修改冲突文件。在冲突文件中Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，例如

   >```
   >Git is a distributed version control system.
   >Git is free software distributed under the GPL.
   >Git has a mutable index called stage.
   >Git tracks changes of files.
   ><<<<<<< HEAD
   >Creating a new branch is quick & simple.
   >=======
   >Creating a new branch is quick AND simple.
   >>>>>>>> feature1
   >```

3. 提交。`master`分支和`feature1`分支变成了下图所示

   ![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

   用带参数的`git log`也可以看到分支的合并情况

   ```
   $ git log --graph 
   $ git log --graph --pretty=oneline --abbrev-commit % (好看的)
   ```

4. 删除分支

   ```
   $ git branch -d f<name>
   ```


### 分支管理策略

#### 合并后显示分支信息

合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

```
$ git merge --no-ff -m "merge with no-ff" dev
```

+ `--no-ff`参数，表示禁用`Fast forward`：
+ 本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去

合并后，我们用`git log`看看分支历史

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

![git-no-ff-mode](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

#### 常见分支策略

+ `master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活
+ 干活都在`dev`分支，版本发布时，再把`dev`分支合并到`master`上，再在`master`发布
+ 每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

## bug分支

每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，当前正在`dev`上进行的工作还没有提交，工作只进行到一半，还没法提交。

Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
git stash
```

用`git status`查看工作区，就是干净的。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支。修复bug，然后提交

```
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支

接着回到`dev`分支干活

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

+ 用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

  ```
  git stash apply
  git stash drop
  ```

+ 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

  ```
  git stash pop
  ```

再用`git stash list`查看，就看不到任何stash内容了：

```
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```

在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。

为了方便操作，Git专门提供了一个`cherry-pick`命令`git cherry-pick <commit>`，让我们能复制一个特定的提交到当前分支

```
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

## feature分支删除

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

```
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

## 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```
$ git remote
origin
```

或者，用`git remote -v`显示更详细的信息：

```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

### 推动分支：

```
$ git push origin master
```

```
$ git push origin dev
```

+ `master`分支是主分支，因此要时刻与远程同步；
+ `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

+ bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
+ feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发

### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支

```
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送。

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```
$ git push origin dev

error: failed to push some refs 
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
```

```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接

```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push。

---

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

# 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag）。

tag是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

>“请把上周一的那个版本打包发布，commit号是6a5819e...”
>
>“请把上周一的那个版本打包发布，版本号是v1.2”

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起

## 创建标签

`git tag <name>`就可以打一个新标签：

```
$ git tag v1.0   % 打在最新提交的commit上
$ git tag v0.9 f52c633  %打在指定commit上
```

创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字

```
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

可以用命令`git tag`查看所有标签（标签不是按时间顺序列出，而是按字母排序的）：

```
$ git tag
```

用`git show <tagname>`查看标签信息：

```
$ git show v0.9
```

## 操作标签

创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除

```
$ git tag -d v0.1
```

推送某个标签到远程，使用命令`git push origin <tagname>`：

```
$ git push origin v1.0
```

一次性推送全部尚未推送到远程的本地标签：

```
$ git push origin --tags
```

删除远程标签就麻烦一点，先从本地删除`git tag -d <tagname>`，然后，从远程删除`git push origin :refs/tags/<tagname>`

```
$ git tag -d v0.9		% 本地删除
$ git push origin :refs/tags/v0.9	% 远程删除
```

# 自定义git

让Git显示颜色，会让命令输出看起来更醒目：

```
$ git config --global color.ui true
```

## 忽略特殊文件

+ 有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们

  Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去

+ 有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了。如果你确实想添加该文件，可以用`-f`强制添加到Git：

  ```
$ git add -f App.class
	```
	
+ `.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：

	```
	$ git check-ignore -v App.class
	.gitignore:3:*.class	App.class
	```
	
	Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

## 配置别名

如果敲`git st`就表示`git status`那就简单多了

```
$ git config --global alias.st status
```



甚至还有人丧心病狂地把`lg`配置成了：

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

来看看`git lg`的效果：

![git-lg](https://www.liaoxuefeng.com/files/attachments/919059728302912/0)

## 配置文件

加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用

配置文件放哪了？

当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中

每个仓库的Git配置文件都放在`.git/config`文件中。

```
$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1
```

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。