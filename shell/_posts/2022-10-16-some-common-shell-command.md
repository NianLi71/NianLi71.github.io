---
layout: post
title:  "常用shell 命令"
date:   2022-10-16 02:09:00 +0800
categories: shell
---

## 常用shell 命令

## ls
```bash
-a all
-d 显示文件夹本身,而不是其中内容
-l 长格式
-r 逆序
-S 按照文件大小排序
-t 按照修改日期排序
-i --inode print the index number of each file

ls -al #默认字母序
ls -alht
ls -alhS
ls -alhSr
ls -li

ls /bin /usr/bin # 列出多个文件或文件夹

ls -d /Users/linia # 列出文件夹本身
```

## cd
```bash
cd
cd ~
cd -

cd $(ls | grep -i event)

% ls | grep -i event
$ cd $(!!) # 进入上一条命令输出的文件夹
```

## pwd
```bash
pwd
pwd -P #展开符号链接
```

## du
查看文件大小
```bash
% du -sh # summary
% du -sh directry_or_file
% du -hd 1 # 分别显示一层深度目录大小
```

## df
查看磁盘空间
```bash
df -h
```

## free
查看内存空间,macOS不适用
```bash
free -h
```

## date
时间日期
```bash
date
```

## cal
日历
```bash
cal
```

## 退出shell
exit or Ctrl-D

## file
查看文件类型
```bash
➜  ~ file sicp.pdf
sicp.pdf: PDF document, version 1.5
```

## less
less is more
查看文件

# 操作文件和目录
## mkdir
创建目录
```bash
mkdir dir1
mkdir dir1 dir2 dir3
```

## cp
```bash
-i --interactive
-r --recursive
-v --verbose

cp file1 file2
cp -i file1 file2
cp file1 file2 dir1
cp dir1/* dir2
cp -r dir1 dir2
```

## rm
小心使用rm,特别是rm -rf 

## mv移动和重命名文件

## tail
```bash
tail -f -n 500
```

## ps查看进程
```
ps -ef
```


## type显示命令类型/which显示命令位置

## alias
* 可以用分号作为多个命令分隔符


```bash
cd /etc; ls -al; cd -

linian@AliyunEcs ~/s/playground> type foo
foo not found
linian@AliyunEcs ~/s/playground> alias foo='cd /etc; ls -al; cd -'

查看系统alias
> alias
```

## sort/uniq

#### unqi
* -d Only output lines that are repeated in the input.
```bash
$ ls /bin /usr/bin | sort | uniq | less # 排序去重

$ ls /bin /usr/bin | sort | uniq -d | less # 显示重复的行

$ (printenv; set) | sort | uniq -d
```

## wc
wc - print newline, word, and byte counts for each file
-c byte counts
-l line counts
-w word counts

```bash
$ ls -l /usr/bin | wc
 862    8101   55179
```
862 newlines
8101 words
55179 bytes

```bash
ls -l /usr/bin | wc -l #统计行数

ls /bin /usr/bin | sort | uniq | wc -l
```

## grep
* 输出与模式匹配的行
* grep会从输入文件中搜索匹配模式,如果没有指定文件,那么会将stdin作为输入, 参看man grep
* 常用的参数
    * -i --ignore-case 
    * -v --invert-match 
    * -c --count 统计匹配行数
* 上下文匹配
    * -B NUM before, 显示匹配行之前NUM行
    * -A NUM after
    * -C NUM 显示前后NUM行
```bash
grep pattern filename

$ ls /bin /usr/bin > ls-output.txt
$ grep 'zsh' ls-output.txt

$ ls /bin /usr/bin | sort | uniq | grep zsh

$ ls /usr/bin | grep -c zip
```

一些实战
```bash
zgrep -c "Fault=1" ser*

zgrep -B 35  "Fault=1" ser* | grep --color "RequestId"

zgrep -c "reqSuccess=0" relay_service_log.2021-10-21-00.fbar-6ub-p-f-1-1*
```

## head/tail
```bash
$ head ls-output.txt
$ head -n 5 ls-output.txt
$ ls /bin /usr/bin | head -5

$ tail -f ls-output.txt # -f --follow 持续观察
```

## tee
Copy standard input to each FILE, and also to standard output.
tee读取stdin,同时写入指定文件和stdout
```bash
$ ls /usr/bin | tee ls.txt | grep zip
```

## printenv
只显示环境变量
```bash
% printenv | less

# 查看环境变量
$ printenv USER
$ echo $USER
```
## set
显示shell变量和环境变量
```bash
➜  foo='yes'
➜  printenv | grep foo
➜  set | grep foo
foo=yes
```

## history
```bash
% history | less
    1  pwd
    2  exit
    3  pwd
    4  vim /bin/zsh
    5  ls -al /bin/zsh
    ...
  
% !5 #重用编号5的history

% !! #重复执行最后一个命令
```

## 运行多条命令
```bash
% ls;\
ls -a;\
ls -al
```
