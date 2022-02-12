---
title: conda
date: 2020-11-15 17:02:48
tags: conda
categories:
---

# conda 基本命令

```python
# 环境
conda create -n xxxx (python = 3.5)  # 创建
conda remove -n xxxx --all # 删除
source activate xxxx # 激活
source deactivate # 退出
conda env list # 所有环境
conda info -e # 确认当前环境

# 包管理
conda list # 查看在当前环境中，已安装的包和对应版本
conda list -n xxxx # xxxx环境下的包
conda install (-n xxxx) x # 在xxxx安装x
conda update x / conda update
conda remove x
conda search x # 查找可安装的包
conda clean -p # 删除没有用的包
conda clean -y all # 删除所有的包和cache
```

+ `conda update conda   `  升级

+ 镜像

  ```
  # 添加Anaconda的TUNA镜像
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  conda config --set show_channel_urls yes
  ```

