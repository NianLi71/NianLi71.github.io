---
layout: post
title:  Shell Loops
date:   2022-11-06 01:05:00 +0800
categories: shell
---

## for
### 迭代每一个值
```bash
#!/bin/bash
for dog in QiuQiu GuoGuo PiQiu XiaoHua
do
    echo "little dog's name is $dog"
done
```
output
```
➜  shell_scripts ./shell_test.sh
little dog's name is QiuQiu
little dog's name is GuoGuo
little dog's name is PiQiu
little dog's name is XiaoHua
```

### 从变量中读取列表
```bash
dogs="QiuQiu GuoGuo"
dogs=$dogs" PiQiu"" XiaoHua"
echo $dogs

for dog in $dogs
do
    echo "little dog's name is $dog"
done
```
output
```bash
➜  shell_scripts ./shell_test.sh
QiuQiu GuoGuo PiQiu XiaoHua
little dog's name is QiuQiu
little dog's name is GuoGuo
little dog's name is PiQiu
little dog's name is XiaoHua
```

### 从命令中读取值
```bash
#!/bin/bash

file='dog_names'

IFS=$'\n'  # 临时改变字段分隔符
for dog in $(cat $file)
do
    echo "little dog's name is $dog"
done
```
output
```bash
➜  shell_scripts cat dog_names 
QiuQiu
GuoGuo
XiaoHua
Xiao PiQiu
➜  shell_scripts ./shell_test.sh
little dog's name is QiuQiu
little dog's name is GuoGuo
little dog's name is XiaoHua
little dog's name is Xiao PiQiu
```

### 用通匹配符读取目录
```bash
#!/bin/bash

for file in /Users/linia/Documents/Codes/shell_scripts/* /Users/linia/Documents/Codes/hello
do
    if [ -d "$file" ]
    then
        echo "$file is a directory"
    elif [ -f "$file" ]
    then
        echo "$file is a file"
    else
        echo "$file doesn't exist"
    fi
done
```
output
```bash
➜  shell_scripts ./shell_test.sh
/Users/linia/Documents/Codes/shell_scripts/dog_names is a file
/Users/linia/Documents/Codes/shell_scripts/shell_test.sh is a file
/Users/linia/Documents/Codes/shell_scripts/test1.sh is a file
/Users/linia/Documents/Codes/shell_scripts/test_dir is a directory
/Users/linia/Documents/Codes/hello doesn't exist
```

## 数字区间迭代
### 迭代一个区间
```bash
for i in {0..10}
do
    echo $i
done
```
```
➜  shell_scripts ./shell_test.sh
0
1
2
3
4
5
6
7
8
9
10
```

### C语言风格for
```bash
#!/bin/bash

for ((a = 1; a < 10; a++ ))
do
    echo "the number is $a"
done
```
output
```bash
➜  shell_scripts ./shell_test.sh
the number is 1
the number is 2
the number is 3
the number is 4
the number is 5
the number is 6
the number is 7
the number is 8
the number is 9
```

#### 迭代多个变量
```bash
#!/bin/bash

for ((a = 1, b = 10; a < 10; a++, b-- ))
do
    echo "$a - $b =" $[a - b]
done
```
output
```bash
➜  shell_scripts ./shell_test.sh
1 - 10 = -9
2 - 9 = -7
3 - 8 = -5
4 - 7 = -3
5 - 6 = -1
6 - 5 = 1
7 - 4 = 3
8 - 3 = 5
9 - 2 = 7
```

## while
### 基本while
```bash
#!/bin/bash

var1=10
while [ $var1 -gt 0 ]
do
    echo $var1
    var1=$[var1 - 1]
done
```
output
```bash
➜  shell_scripts ./shell_test.sh
10
9
8
7
6
5
4
3
2
1
```

## 实例
### 用来查找PATH中可执行文件
```bash
#!/bin/bash

IFS=: # 用来分隔PATH中的值
for folder in $PATH
do
    echo $folder
    for file in $folder/*
    do
        if [ -x $file ]
        then
            echo "    $file"
        fi    
    done
done
```