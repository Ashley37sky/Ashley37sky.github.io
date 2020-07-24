---
title: coursera/github慢
date: 2020-07-15 02:39:14
tags: hacks
---

# methods

<!--more-->

1. 查找域名对应的ip地址

   > github:
   >
   > + github.com
   >
   > + github.global.ssl.fastly.net 

   > coursera:
   >
   > + d3c33hcgiwev3.cloudfront.net

   域名解析网站

   + https://www.ipaddress.com/
   + http://ping.chinaz.com/d3c33hcgiwev3.cloudfront.net

2. 修改hosts `C:\Windows\System32\drivers\etc\hosts` 文件

   > <ip> <域名>

   ```
   87.248.214.183 static3.cdn.Ubi.com
   95.140.224.199 static2.cdn.Ubi.com
   87.248.214.183 static1.cdn.Ubi.com
   221.110.130.114 uplaypc-s-ubisoft.cdn.ubi.com
   221.110.130.114 uplaypc-s-ubisoft-ww.cdn.ubi.com
   
   
   140.82.113.3 github.com
   199.232.69.194  github.global.ssl.fastly.net 
   
   52.84.167.78 d3c33hcgiwev3.cloudfront.net
   13.224.164.137 d3c33hcgiwev3.cloudfront.net
   13.224.164.148  d3c33hcgiwev3.cloudfront.net
   99.84.194.187  d3c33hcgiwev3.cloudfront.net
   52.84.186.109  d3c33hcgiwev3.cloudfront.net
   54.230.85.123 d3c33hcgiwev3.cloudfront.net
   13.224.164.137    d3c33hcgiwev3.cloudfront.net
   14.13.224.164.148    d3c33hcgiwev3.cloudfront.net
   15.13.224.164.208    d3c33hcgiwev3.cloudfront.net
   16.13.224.164.173   d3c33hcgiwev3.cloudfront.net
   17.31.13.78.66    d3c33hcgiwev3.cloudfront.net
   ```

   (混入奇奇怪怪的东西)

3. cmd中使用`ipconfig\flushdns`

# why（填坑？？）

`hosts`:其作用就是将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，当用户在浏览器中输入一个需要登录的网址时，系统会首先自动从Hosts文件中寻找对应的IP地址，一旦找到，系统会立即打开对应网页，如果没有找到，则系统会再将网址提交DNS域名解析服务器进行IP地址的解析。
需要注意的是，Hosts文件配置的映射是静态的，如果网络上的计算机更改了请及时更新IP地址，否则将不能访问。

`dns`: ????