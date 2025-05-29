# 玩转Linux操作系统

## Linux概述
Linux是一个通用操作系统。一个操作系统要负责任务调度、内存分配、处理外围设备I/O等操作。
操作系统通常由内核（运行其他程序，管理像磁盘、打印机等硬件设备的核心程序）和系统程序（设备驱动、底层库、shell、服务程序等）两部分组成。

**基础命令**

```
-history # 查看历史会话
```

**实用程序**
1. 文件和文件夹操作 - mkdir/rmdir
```
[root ~]# mkdir abc # 创建文件夹
[root ~]# mkdir -p xyz/abc # -p 一次性创建多层文件
[root ~]# rmdir abc # 删除文件夹
```
2.  创建/删除文件 - touch / rm。
```
[root ~]# touch readme.txt
[root ~]# touch error.txt
[root ~]# rm error.txt
rm: remove regular empty file ‘error.txt’? y
[root ~]# rm -rf xyz
```
`touch`有两个功能：
(1). 是用于把已存在文件的时间标签更新为系统当前的时间（默认方式），它们的数据将原封不动地保留下来；
(2). 是用来创建新的空文件。

3. 拷贝，移动文件 -cp/mv。
4. 查看目录内容： -ls
     - `-l`: 以长格式查看文件和目录
     - `-a`: 显示以点开头的文件和目录（隐藏文件）。
     - `-R`: 遇到目录要进行递归展开（继续列出目录下面的文件和目录）
     - `-d`: 只列出目录，不列出其他内容。
     - `-S`/`-t`: 按大小/时间排序。
  
5. 切换和查看当前工作目录 - cd / pwd。
6. 查看文件内容 - cat / tac / head / tail / more / less / rev / od。
7. 文件重命名 - rename。
8. 查找文件和查找内容 - find / grep。
9. 压缩/解压缩和归档/解归档 - gzip / gunzip / xz。
10. 将标准输入转成命令行参数 - xargs。

**管道和重定向**

1. 重定向：

有些时候想要保存某个命令的输出,而不仅仅只是让它显示在显示器Q上。bash shell提供了几个操作符,可以将命令的输出重定向到另一个位置(比如文件)。重重定向可以用于输入,也可以用于输出,可以将文件重定向到命令输入。

- 输出重定向：将命令的输出发送到一个文件中，用`>`完成
- 
  注意，使用命令`command -> outputfile`，会覆盖`outputfile`之前的内容
```
[root ~]# cat readme.txt
banana
apple
grape
apple
grape
watermelon
pear
pitaya
[root ~]# cat readme.txt | sort | uniq > result.txt
[root ~]# cat result.txt
apple
banana
grape
pear
pitaya
watermelon
```

- 输入重定向：给定linux命令一个输入来源来取代标准输入，对输入来源进行处理，然后输出到标准输出，用`<`

2. 管道：

用于进程间的通信，是将前面每一个进程的输出直接作为下一个进程的输入

例如：查找当前目录下文件的个数
```
[root ~]# find ./ | wc -l
6152
```

