---
title: coursera批量下载
date: 2020-07-15 02:54:49
tags: hacks
---


> https://github.com/LoganDing/Coursera.org-Downloader

# methods

<!--more-->

1. 进入python安装目录下`Scripts` ，并且将路径添加到系统path中

   `我的电脑->属性->高级系统设置->高级->环境变量->系统变量->path->新建`

2. 安装包

   ```
   pip install coursera-dl
   ```

3. 运行脚本

   ```
   coursera-dl -u <user> -p <pass> <coursename>
   ```

   *course_name一般在URL中learn/course_name*
   
4. 若出现无法登录问题，添加cauth

   ```
coursera-dl -u <user> -p <pass> --cauth <cauth> <coursename>
   ```
   
   如何获取cauth？`f12->application->cookies(https://www.coursera.org/)->CAUTH`
   
5. 其他操作

   ```
   Without -p field:            coursera-dl -u <user> modelthinking-004
   Multiple classes:            coursera-dl -u <user> -p <pass> saas historyofrock1-001 algo-2012-002
   Filter by section name:      coursera-dl -u <user> -p <pass> -sf "Chapter_Four" crypto-004
   Filter by lecture name:      coursera-dl -u <user> -p <pass> -lf "3.1_" ml-2012-002
   Download only ppt files:     coursera-dl -u <user> -p <pass> -f "ppt" qcomp-2012-001
   Use a ~/.netrc file:         coursera-dl -n -- matrix-001
   Get the preview classes:     coursera-dl -n -b ni-001
   Download videos at 720p:     coursera-dl -n --video-resolution 720p ni-001
   Specify download path:       coursera-dl -n --path=C:\Coursera\Classes\ comnetworks-002
   Display help:                coursera-dl --help
   
   Maintain a list of classes in a dir:
   Initialize:              mkdir -p CURRENT/{class1,class2,..classN}
   Update:                  coursera-dl -n --path CURRENT `\ls CURRENT`
   ```

   回复下载

   ```
   coursera-dl -u <user> -p <pass> --resume sdn1-001
   ```

   *`CTRL+C`, partially downloaded files will be deleted from your disk and you have to start the download process from the beginning, other interruptions can use the restore*

# why？？（填坑）

`pip`：

`cookie`

`cauth`

`f12`

`批处理`