---
comments: true
date: 2010-10-10 17:43:09
layout: post
slug: building-mysqldb-for-python-on-windows
title: 在Windows编译MySQLdb for python
wordpress_id: 610
categories:
- Programing
tags:
- MySQL
- Python
- Windows
---

之前倒是在ubuntu上用过，双系统在win下面工作，出于懒得重启这么个原因于是想快速在win上跑一段python代码，想想是挺快的一件事情结果倒还真是折腾了一段时间。在[sourceforge](http://sourceforge.net/projects/mysql-python/)上下载了最新版（1.2.3），开始安装，于是在执行`python setup.py install`的时候先后遇到了三个问题，分别记录如下：




* * *




**问题**：

```
_mysql.c(34) : fatal error C1083: Cannot open include file: 'config-win.h': No such file or directory
error: command '"C:Program FilesMicrosoft Visual Studio 9.0VCBINcl.exe"' failed with exit status 2
```



**原因**：原因是安装MySQL的时候没有安装C语言库。




**解决**：重新运行MySQL的安装程序，选择Modify，把`C Include Files / Lib Files`勾选上，并安装。




* * *




**问题**：

```
Traceback (most recent call last):
 File "setup.py", line 15, in <module>
 metadata, options = get_config()
 File "C:MySQL-python-1.2.3setup_windows.py", line 7, in get_config
 serverKey = _winreg.OpenKey(_winreg.HKEY_LOCAL_MACHINE, options['registry_key'])
WindowsError: [Error 2] The system cannot find the file specified
```


**原因**：MySQL for python 1.2.3仍然是在寻找MySQL5.0的版本




**解决**：

1、打开目录下site.cfg文件，修改最后一行为`registry_key = SOFTWAREMySQL ABMySQL Server 5.1`

2、打开setup_windows.py文件，修改第七行为`serverKey = _winreg.OpenKey(_winreg.HKEY_LOCAL_MACHINE, 'SOFTWAREMySQL ABMySQL Server 5.1')`




* * *




**问题**：

```
buildtemp.win32-2.7Release_mysql.pyd.manifest : general error c1010070: Failed to load and parse the manifest. The system cannot find the file specified.
error: command 'mt.exe' failed with exit status 31
```




**原因**：路径发生变化？




**解决**：打开`你的PYTHON安装目录Libdistutilsmsvc9compiler.py`文件，找到`ld_args.append('/MANIFESTFILE:' + temp_manifest)`这行代码，将其改为`ld_args.append('/MANIFEST')`




* * *




这样应该就可以正常`python setup.py install`了。




以上。
