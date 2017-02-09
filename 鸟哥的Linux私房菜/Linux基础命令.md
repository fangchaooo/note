# 命令

## command [-option] parameter1 parameter2...


帮助
  - `xx --help`
  - `man xx`

`cd` 路径跳转

  - `cd ..`返回上一层路径
  - `cd /a/b`绝对路径跳转

`ls`  显示当前路径下的文件

  - `ls -l` 显示文件权限，列表显示
  - `ls -a` 显示文件所有子目录和文件
  - `ls -h` 显示文件大小
  - `ls xx* ls *xx` 文件名的匹配
  - `ls x?x` 文件名匹配（只匹配一个字符）
  - `ls [abc]`匹配a,b,c中一个
  - `ls [a-c]*`匹配a-c中的一个
  - `ls \*a`查找文件名为`*a`的文件（转义字符）


`pwd` 显示当前路径

`>`重定向


`>>`末尾添加

`more`分屏

`|`管道


创建
  - `tree`查看文件目录结构

  - `touch`创建文件

  - 'touch a\xx.py'在a下创建xx.py文件

  - `mkdir`创建文件夹

  - `mkdir a/b/c -p`创建层次文件

删除

  - `rm`删除

    1. `-f`强制删除
    2. `-r`删除文件夹必须加


  - `rmdir`删除空文件夹

`cat` 查看文件

`cp a b`拷贝文件a内容到b

  - `-v`显示进度
  - `-r`先创建b，在执行命令，复制整个文件夹a到b

`mv A B` 移动 a 到 b，也可以改名

`cal`日历

`date 月日时分年.秒`设置时间

`sudo`管理员命令

`ps`查看当前系统进程

  - `-aux`全部显示

`kill`杀死进程

`top`动态显示进程

`reboot`重启

`shutdown`关机

  - `-r now`
  - `-h now`
  - `-h xx:xx`
  - `-h +xx`

`init 0 6`关机/重启

`df`检查磁盘情况

`du`检测目录所占磁盘情况

`ping`

`ifconfig`

`grep` 查找文本

  - `-n 'xx' 文件名`显示文件中包含xx的文字行数及这行的文本

  - `i` 忽略大小写

`find`查找文件

  - `find ./ -name xxx.xx`查找当前目录下名为xxx.xx的文件

  - `find ./ -name '*.xx'`查找到目录下所有后缀为xx的文件


压缩文件

  - `tar -cvf xx`打包此路径下文件，名为xx（不删除路径下文件）

  - `tar -xvf xx`解包

  - `gzip xx.tar`压缩为xx

  - `gzip -d xx.tar.gz`解压缩

  - `tar zcvf xxx.tar.gz 文件`压缩并打包

  - `tar zxvf xxx.tar.gz`解压解包

  - `tar jcvf xxx.tar.bz2 文件`压缩并打包

  - `tar jxvf xxx.tar.bz2`解压解包

  - `zip xxx.zip 文件`zip式压缩

  - `unzip -d xxx xxx.zip`解压xxx.zip到xxx文件夹

`ln -s xx xxx`软链接

`which` 命令路径
