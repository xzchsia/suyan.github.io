---
layout: post
title: mysql插入表中的中文显示为乱码或问号的解决方法  
category: 技术
tags: [MySQL, 中文乱码, 编码]
keywords: MySQL, 中文乱码, 编码  
description: mysql插入表中的中文显示为乱码或问号的解决方法  
---


在做项目的时候发现mysql数据库中的中文显示为问号，产生乱码的主要原因是因为选用的编码不对或者编码不一致造成的。

参考原文章地址: 
[https://www.cnblogs.com/houqi/p/5713176.html][orignal-paper]


**mysql中文显示乱码或者问号是因为选用的编码不对或者编码不一致造成的**

可以先查看下未修改前的编码格式，使用命令行查看

`show variables like 'char%';  显示编码格式`

![character-before][character-before]
未修改my.ini配置文件的编码文件latin1（即ISO-8859-1）

可以看到编码格式并不全是utf8的编码格式，这个就是产生中文等字符乱码的根本原因。

### 配置文件my.ini的修改  

通过修改my.ini配置文件(Windows系统)。（配置文件在安装的根目录下如下图）

![My.ini][myini]

我的Windows 7系统的默认路径是在C:\ProgramData\MySQL\MySQL Server 5.7这个目录下

找到 `default-character-set` 和 `character-set-server` 这两个配置文件，在我的生产环境中5.7的配置文件中，是使用\#号注释掉的，去掉\#注释，同时修改指为utf8即可,
之后使用Navicat连接MySQL数据库，创建一个数据库编码格式选择utf8，排序方式的 `collation-server` 选择 `utf8_general_ci`  

*注意：有的版本不支持 `default-character-set=utf8` ，用 `character_set_server=utf8` 来取代  `default-character-set=utf8` 即可)* 

![myini-modify][my.ini-modify]


### 重启mysql服务
在Windows平台可以通过下面两种方式来处理

- [x] 计算机---->右键--->管理---->服务和应用程序--->服务--->找到mysql即可  
- [x] 命令行command模式 

以管理员身份运行cmd.exe，进行如下操作。

- 关闭服务 : `net stop mysql`  

- 开启服务 : `net start mysql`

注意服务名称，不一定是mysql，在5.7中，默认的是mysql57这个服务名称

![service-cmd][service-cmd]

### 查看编码格式。还是在cmd中

1. 输入 `mysql -u root -p`      进入mysql数据库 

2. 键入密码：*****（自己的密码，没有的话直接回车键，嗯其他情况如忘了root密码百度去orz。。）

3. `show variables like 'char%';`  显示编码格式

下图为已经修改过的。

![character-after][character-after]
图二（已修改编码）

可以看出都已经更正为utf8了，这样新建立的数据库缺省就是UTF8编码了


### 彩蛋：

然后你以为到这就完了么，并没有。。接下来你会发现报这个错如下所示。

![error-info][error-info]

上述错误是什么引起的呢，还是因为编码不正确啊！因为使用了已经创建好的数据库和表但没有更改为utf-8；
通过以下命令查看表的编码为Latin1：

`show create table tablename（数据库名.表名）;`

![show-table-character][show-table-character]

### 修改方法：

<code> ALTER DATABASE \`数据库\` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci </code>

<code>ALTER TABLE \`数据表\` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci</code>

(注：此句把表默认的字符集和所有字符列（CHAR,VARCHAR,TEXT）改为新的字符集：)

![table-character][table-character]

![filed-character][filed-character]

### 数据库编码的修改和查询

![show-db-character][show-db-character]

## 总结：

为了全面的支持中文等GBK之类的文字，要更改数据库的默认字符集为utf8，更改表的字符集为utf8，更改列的字符集为utf8，然后重新启动MYSQL服务；最后大功告成！

![ok-sup-lang][ok-sup-lang]




[orignal-paper]:https://www.cnblogs.com/houqi/p/5713176.html
[myini]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160727234329825-116059624.png 
[my.ini-modify]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160727235441372-2013825169.png  
[service-cmd]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728000724903-2115750385.png  
[character-before]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728002218231-227808784.png
[character-after]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728002925513-232106783.png
[error-info]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728001125966-885159927.png
[show-table-character]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728005603872-492461123.png
[table-character]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728010253950-463010496.png
[filed-character]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728010641888-218387532.png
[show-db-character]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728005213138-1510616923.png
[ok-sup-lang]:https://images2015.cnblogs.com/blog/986643/201607/986643-20160728010906669-807329471.png
