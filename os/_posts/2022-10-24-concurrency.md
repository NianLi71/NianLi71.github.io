---
layout: post
title:  "OS Three Pieces - Concurrency"
date:   2022-10-24 02:15:00 +0800
categories: os concrrency
---

## [Operating Systems: Three Easy Pieces][Book link] 

### [25 Dialogue](https://pages.cs.wisc.edu/~remzi/OSTEP/dialogue-concurrency.pdf)
Give a concrete multi-thread example with peaches. It's fun to take a look.

### [26 Concurrency and Threads][26 Concurrency and Threads]

* thread is very much like a separate process, except for one difference: they share the **same** address space and thus can access the same data.
* The state of a single thread is thus very similar to that of a process.
It has a program counter (**PC**) that tracks where the program is fetching instructions from. 
* One other major difference between threads and processes concerns
the **stack**. 
* However, in a multi-threaded process, each thread runs independently
and of course may call into various routines to do whatever work it is doing. Instead of a single stack in the address space, there will be one per
thread.
* In this figure, you can see *two stacks spread throughout the address
space of the process.*
![Figure26_1](/os/images/Figure26_1.png)

[Book link]: https://pages.cs.wisc.edu/~remzi/OSTEP/#book-chapters
[26 Concurrency and Threads]: https://pages.cs.wisc.edu/~remzi/OSTEP/threads-intro.pdf