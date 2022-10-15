---
layout: post
title:  "shell 重定向与管道"
date:   2022-10-16 02:09:00 +0800
categories: shell
---

# 重定向
参考"Linux命令行大全"中第章内容
## stdin,stdout,stderr文件描述符
Shell内部对3个标准文件流stdin,stdout,sterr分别用文件描述符0,1,2引用
stdin 0
stdout 1
stderr 2

## stdout标准输出重定向
```bash
ls -l /usr/bin > ls-output.txt
ls -l /usr/bin >> ls-output.txt # 追加写

ls -l /usr/bin 1> ls-output.txt # 使用文件描述符
```

## stderr标准错误重定向


```bash
ls -l /bin/usr 2> ls-error.txt
```

* 将stdout,stderr重定向到同一个文件中:
##### 旧版用法
```bash
ls -l /bin/usr > ls-output.txt 2>&1 # 旧版用法,注意重定向顺序
```
顺序很重要
```bash
ls -l /bin/usr 2>&1 > ls-output.txt #不会按照预期执行,stderr将会输出到屏幕
```

##### 新版用法
```bash
ls -l /bin/usr &> ls-output.txt # 较新版本bash用法

ls -l /bin/usr &>> ls-output.txt # 追加写
```

##### 丢弃输出结果, /dev/null
```bash
ls -l /bin/usr 2> /dev/null
ls -l /bin/usr > ls-output.txt 2> /dev/null # 丢弃stderr, 重定向stdout到文件
```

## stdin标准输入重定向
将文件作为输入,重定向给cat
```bash
$ cat > dog.txt
hello dog
$ less dog.txt
$ cat < dog.txt
hello dog
```

# 管道
通过管道操作符|, 可以将一个命令的标准输出传给另一个命令的标准输入:
command1 | command2


```bash
ls -al /usr/bin | less # 通过less逐页显示结果

ls /bin /usr/bin | sort | less
```

## >和|之间的差异, 重定向与管道的差异
* ```>``` 重定向操作符将命令与文件联系在一起
* ```|``` 管道操作讲一个命令的标准输出结果与另一个命令的标准输入连接在一起

**注意重定向会消无声息地创建或覆盖文件**
例如:
```bash
ls > less
```
会创建写入less文件,如果当前在/usr/bin下会覆盖less命令

## 一些管道的例子
```bash
$ ls /bin /usr/bin | sort | less

$ ls /bin /usr/bin | sort | uniq | less # 排序去重

$ ls /bin /usr/bin | sort | uniq -d | less # 显示重复的行

$ ls -l /usr/bin | wc -l #统计行数

$ ls /bin /usr/bin | sort | uniq | wc -l

```