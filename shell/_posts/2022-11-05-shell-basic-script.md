---
layout: post
title:  Shell Basic
date:   2022-11-05 00:50:00 +0800
categories: shell
---

#### 查看退出状态码
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