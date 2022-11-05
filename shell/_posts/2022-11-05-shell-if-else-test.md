---
layout: post
title:  Shell if-else test
date:   2022-11-05 00:30:00 +0800
categories: shell
---



参考"Linux命令号与shell脚本编程大全(第3版)"第12章


## if-then
if语句会运行if后面的那个命令, **如果该命令的退出状态码是0(该命令成功运行), 则位于then部分的命令就会被执行.** 如果是其他退出状态码, then部分就不会被执行.

```bash
#!/bin/bash

if pwd
then
    echo "It worked"
fi

if NotaCommand
then
    echo "This worked?"
fi
echo "Outside the 'if NotaCommand'"
```
output
```bash
/Documents/Codes/shell_scripts
It worked
./shell_test.sh: line 8: NotaCommand: command not found
Outside the 'if NotaCommand'
```


## if-then-else
```bash
#!/bin/bash

testuser=cute_dog
if grep $testuser /etc/passwd
then
    echo "The user $testuser exists"
    ls -a /home/$testuser
else
    echo "The user $testuser does not exist"
fi
```


## if-then-elif-else
```bash
#!/bin/bash

testuser=cute_dog
if grep $testuser /etc/passwd
then
    echo "The user $testuser exists."
elif ls -d /home/$testuser
then
    echo "The user $testuser does not exist."
    echo "However, $testuser has a directory."
else
    echo "The user $testuser does not exist."
    echo "And, $testuser does not have a directory."
fi
```


## test 命令
如果test命令中列出的条件成立, test命令就会退出并返回退出状态码0.
```bash
if test condition
then
    commands
fi
```

```bash
➜  shell_scripts test pwd
➜  shell_scripts echo $?
0
➜  shell_scripts test $my_var # 变量不存在
➜  shell_scripts echo $?
1
➜  shell_scripts my_var="" # 变量没有内容                   
➜  shell_scripts test $my_var
➜  shell_scripts echo $?
1
➜  shell_scripts my_var="hello"
➜  shell_scripts test $my_var
➜  shell_scripts echo $?
0
```

**test命令另一种形式**
```bash
if [ condition ]
then
    commands
fi
```

test命令可以判断三类条件
* 数值比较
* 字符串比较
* 文件比较

#### 数值比较
![table-12-1](/shell/images/linux-shell-table-12-1.png)

```bash
#!/bin/bash

value1=10
value2=11

if [ $value1 -gt 5 ]
then
    echo "$value1 is greater than 5"
fi

if [ $value2 -eq $value1 ]
then
    echo "values are equal"
else
    echo "values are different"
fi

if [ $value1 -gt 5.55 ] # bash shell 只能处理整数
then
    echo "$value1 is greater than 5.55"
fi
```
output
```bash
10 is greater than 5
values are different
./shell_test.sh:[:18: integer expression expected: 5.55
```

#### 字符串比较
![table-12-2](/shell/images/linux-shell-table-12-2.png)
```bash
➜  shell_scripts testuser=rich
➜  shell_scripts [ $USER = $testuser ] 
➜  shell_scripts echo $?
1

➜  shell_scripts var=hello
➜  shell_scripts [ $var = hello ] 
➜  shell_scripts echo $?
0
```

**注意: <, > 比较需要转义, 否则会被当成重定向符号**
```bash
#!/bin/bash

val1=hello
val2=world

if [ $val1 \> $val2 ]
then
    echo "$val1 is greater than $val2"
else
    echo "$val1 is less then $val2"
fi
```
output
```
hello is less then world
```
或者使用双方括号做比较大小
```
if [[ $val1 > $val2 ]]
```

-n, -z用来检查变量是否含有数据
```bash
➜  shell_scripts val1=testing                                        
➜  shell_scripts val2=""     
➜  shell_scripts [ -n $val1 ] # val1长度是否非0
➜  shell_scripts echo $?
0
➜  shell_scripts [ -z $val2 ] # val2长度是否为0
➜  shell_scripts echo $?
0
➜  shell_scripts [ -z $val3 ] # val3长度是否为0, val3未被定义过
➜  shell_scripts echo $?
0
```

#### 文件比较
文件比较允许你测试Linux文件系统上文件和目录的状态.
![table-12-3](/shell/images/linux-shell-table-12-3.png)

```bash
#!/bin/bash

jump_dir=$HOME/tmp
if [ -d $jump_dir ]
then
    cd $jump_dir
    ls
fi
```

```bash
#!/bin/bash

location=$HOME/tmp
file_name=hello.txt

if [ -e $location ]
then
    if [ -e $location/$file_name ]
    then
        cat $location/$file_name
    fi
fi
```


## 复合条件测试

```bash
[ condition1 ] && [ condition2 ]
[ condition1 ] || [ condition2 ]

➜  shell_scripts [ -d $HOME ] && [ -w $HOME/testing ]; echo $?
1
```

## 使用双括号`((  ))`
双括号命令允许你在比较过程中使用高级数学表达式.
```
(( expression ))
```
*expression*可以使任意的数学赋值或比较表达式
![table-12-4](/shell/images/linux-shell-table-12-4.png)

```bash
➜  shell_scripts val1=10     
➜  shell_scripts ((val1 = $val1 ** 2))               
➜  shell_scripts echo $val1
100
```
```bash
#!/bin/bash

val1=10

if (( $val1 ** 2 > 90 )) # 双括号里大于号不需要转义
then
    (( val2 = $val1 ** 2 ))
    echo "The square of $val1 is $val2."
fi
```



## 使用方双括号`[[  ]]`
* 双方括号提供了针对字符串比较的高级特性.
* 双方括号里的*expression*使用了test命令中采用的标准字符串比较, 但提供了*模式匹配*功能
```
[[ expression ]]
```

注意模式匹配时不能加单双引号
```bash
➜  shell_scripts val=hello
➜  shell_scripts [[ $val == "h*" ]] 
➜  shell_scripts echo $?
1
➜  shell_scripts [[ $val == h* ]]  
➜  shell_scripts echo $?
0
```

```bash
#!/bin/bash

if [[ $USER == l* ]]
then
    echo "Hello $USER."
else
    echo "Sorry, I don't konw you."
fi
```

可以用来判断version是否是指定version
```bash
#!/bin/bash

version=4.68
if [[ $version == 4* ]]
then
    echo "version $version is about 4.x"
fi
```


## 总结
* 标准的test命令是单括号`[]`注意和双方括号区分
* 双圆括号针对数学表达式, 双方括号针对字符串
* 我发现基本上单括号`[]`能用的地方`[[  ]]`都能用, 尝试去发现一些不适用的场景