---

title: git命令
date: 2020-06-20 17:38:30
tags: git
categories: git
---

# 创建

```
git init
```

<!--more-->

```
git add <file>
```

```
git commit -m <message>
```

# 版本回退删除

## 查看

```
git status %查看仓库状态,是否有文件被修改过
```

```
git diff <file> 		  %working tree与index差别
git diff --cached <file>  %index与repository差别
git diff HEAD <file>  	  %查看working tree和repository差别。HEAD代表的是新commit的信息。
```

## 版本回退

```
git log %查看提交日志
git log --pretty=oneline
```

```
git reset --hard HEAD^       %回退到上一个版本
```

```
git reset --hard 1094a      %版本号没必要写全，前几位就可以了，Git会自动去找
```

```
git reflog  %查看每一次命令
```

## 撤销修改

#### 撤销工作区

```
$ git checkout -- <file>  %把在工作区的修改全部撤销
```

#### 撤销暂存区

```
git reset HEAD <file> %把暂存区的修改撤销掉（unstage），重新放回工作区
git checkout -- <file>  %把在工作区的修改全部撤销
```

## 删除&恢复

#### 删除文件

```
git rm test.txt 
git commit -m "remove test.txt"
```

#### 恢复误删文件

```
git checkout -- test.txt
```

# 远程仓库

#### 关联仓库

```
$ git remote add origin 
git@github.com:<github_user_name>/<github_repo_name >.git
```

#### 推送到远程

```
$ git push -u origin master  % 第一次
$ git push origin master
```

#### 克隆

```
git clone git@github.com:m<user_name>/<repo_name>.git
git clone https://github.com/<user_name>/<repo_name>.git
```

# 分支管理

#### 创建切换

```
$ git checkout -b dev 创建并切换
$ git branch dev     % 创建
$ git checkout dev   % 切换
```

```
$ git switch -c dev   %创建并切换
$ git switch master   %创建
```

#### 查看分支

```
$ git branch
```

#### 合并

```
$ git merge dev     %用于合并指定分支到当前分支
$ git merge --no-ff -m "<message>" dev   %合并后显示分支信息
```

#### 删除分支

```
$ git branch -d dev
git branch -D <name   % 强行删除未合并分支
```

#### 储藏现场

```
git stash    
```

```
$ git stash list    %查看工作现场
```

#### 恢复现场

```
git stash apply
git stash apply stash@{0}

git stash drop
```

```
git stash pop
```

#### 复制一个特定的提交到当前分支

```
git cherry-pick <commit>
```

#### 查看远程库的信息

```
$ git remote
$ git remote -v   %详细信息
```

### 推动分支

```
$ git push origin master
$ git push origin dev
```

#### 创建远程`origin`的`dev`分支到本地

```
$ git checkout -b dev origin/dev
```

#### 指定本地`dev`分支与远程`origin/dev`分支的链接

```
$ git branch --set-upstream-to=origin/dev dev
```

# 标签管理

#### 打标签

```
$ git tag v1.0   % 打在最新提交的commit上
$ git tag v0.9 f52c633  %打在指定commit上
$ git tag -a v0.1 -m "version 0.1 released" 1094adb   %指定标签名&说明文字
```

#### 查看所有标签

```
$ git tag
```

#### 查看标签信息

```
$ git show v0.9
```

#### 本地删除

```
$ git tag -d v0.1
```

#### 推送到远程

```
$ git push origin v1.0
$ git push origin --tags    % 推送所有
```

#### 删除远程标签

```
$ git tag -d v0.9		% 本地删除
$ git push origin :refs/tags/v0.9	% 远程删除
```

# 自定义

#### 显示颜色

```
$ git config --global color.ui true
```

#### 忽略特殊文件

```
$ git add -f App.class   %添加已经被忽略的文件
$ git check-ignore -v App.class  %检查ignore
```

#### 配置别名

```
$ git config --global alias.st status   %有空格要''
```

