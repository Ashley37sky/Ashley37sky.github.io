---
title: conda
date: 2020-11-15 17:02:48
tags: conda
categories:
---

# conda

+ `conda update conda   `升级

## 环境管理

+ `conda create --name py34 python=3.4`   创建环境
+ `source activate py34 ` 激活环境
+ `conda info -envis` 确认当前环境

<!--more-->

+ `source deactivate`   退出环境
+ `conda remove -n py34 --all` 删除环境
+ `conda info -e` 查看已有环境

## 包管理

+ 镜像

  ```
  # 添加Anaconda的TUNA镜像
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  conda config --set show_channel_urls yes
  ```

  

+ `conda list`查看在当前环境中，已安装的包和对应版本

+ `conda search X`	 查找可安装的包

+ `conda install X`

+ `conda update X`

+ `conda remove X`

 