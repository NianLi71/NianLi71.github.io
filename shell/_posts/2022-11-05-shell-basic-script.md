---
layout: post
title:  Shell Basic
date:   2022-11-05 00:15:00 +0800
categories: shell
---


参考"Linux命令号与shell脚本编程大全(第3版)"第11章

## 查看当前shell
```bash
➜  shell_scripts echo $SHELL
/bin/zsh
```

## 使用多个命令
```bash
#第一种方式, 在同一行输入所有命令
➜  shell_scripts date; who

#第二种方式, 在多行输入并使用\连接多行
date;\  
who
```


## 创建shell 脚本
* 在文件第一行指定shell
```sh
#!/bin/bash
```

* add permission
```sh
chmod u+x test1.sh
```
* run script
```
./test1.sh
```

## 引用变量
```bash
echo $HOME
echo ${HOME}
```

## 命令替换
命令替换允许你将shell命令的输出赋给变量
```bash
➜  shell_scripts testing=$(date)
➜  shell_scripts echo $testing  
Sun Nov  6 00:40:07 CST 2022

➜  shell_scripts today=$(date +"%y%m%d")
➜  shell_scripts echo $today     
221106
➜  shell_scripts ls -al $HOME > log.$today
➜  shell_scripts ls log.221106            
log.221106
```

```bash
#!/bin/bash

# testing=`date`
testing=$(date)
echo "The date and time are: " $testing
```


## 数学运算
#### 使用方括号
```bash
➜  shell_scripts var1=$[1+5]         
➜  shell_scripts echo $var1
6
➜  shell_scripts var2=$[$var1 * 2]
➜  shell_scripts echo $var2
12
```

#### 使用双圆括号
```sh
➜  shell_scripts val1=$(( 4*5 ))
➜  shell_scripts (( val2 = 4 * 5 ))
➜  shell_scripts printf "%6.3f\n" $val1 $val2
20.000
20.000
```
**如果使用zsh**的话可以在双圆括号里处理浮点数. 注意第一行`#!/bin/zsh`
```sh
#!/bin/zsh

val1=$(( 4*5.1 ))
(( val2 = 4 * 5.1 ))
printf "%6.3f\n" $val1 $val2
```
output
```sh
➜  shell_scripts ./shell_test.sh
20.400
20.400
```

## 查看退出状态码
```bash
➜  shell_scripts pwd
/Documents/Codes/shell_scripts
➜  shell_scripts echo $?
0
➜  shell_scripts ppp
zsh: command not found: ppp
➜  shell_scripts echo $?
127
```

#### exit命令指定退出状态码
```sh
#!/bin/bash

exit 5
```
output
```sh
➜  shell_scripts ./shell_test.sh
➜  shell_scripts echo $?
5
```