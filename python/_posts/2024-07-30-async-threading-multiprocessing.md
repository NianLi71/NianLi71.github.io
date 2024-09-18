---
layout: post
title:  "Async vs Threading vs Multiprocessing in Python"
date:   2024-07-29 00:00:00 +0800
categories: python async threading multiprocessing
---

# Concurrent, Parallel
* Concurrent, 并发: 拿单线程来说, 在某一时刻只有一个task在执行, 但是在一定时间片内, 多个task都得到了一定程度的执行机会
* Parallel, 并行: 在某一时刻, 多个task都在执行
* Python因为有GIL的存在, Python中的multithreading不是parallel执行, 是concurrent并发执行, Python中multiprocessing才是真的parallel running on multiple CPU
* Java中的multithreading是parallel running

https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism
> **Concurrency** is when two or more tasks can start, run, and complete in overlapping time periods. It doesn't necessarily mean they'll ever both be running at the same instant. For example, multitasking on a single-core machine.
>
> **Parallelism** is when tasks literally run at the same time, e.g., on a multicore processor.



# Async vs Threading vs Multiprocessing in Python
* https://rawheel.medium.com/async-vs-threading-vs-multiprocessing-in-python-e35dd69c9696


# Practical Guide to Asyncio, Threading & Multiprocessing
* https://itnext.io/practical-guide-to-async-threading-multiprocessing-958e57d7bbb8#:~:text=So%20when%20to%20use%20which%3F


## Major Differences
Now that we know the options, we discuss a bit about the major differences:

* **Synchronous vs others:** In synchronous execution, we can decide which order everything is run. **In async, threading and multi-processing we leave it to the underlying system to decide.**

* **Multiprocessing vs others:** Multiprocessing is the only one that is really runs multiple lines of code at one time. Async and threading sort of fakes it. However, async and threading can run multiple IO operations truly at the same time.

* **Asyncio vs threading:** Async runs one block of code at a time while threading just one line of code at a time. With async, we have better control of when the execution is given to other block of code but we have to release the execution ourselves.

Note also that due to GIL (Global Interpreter Lock), **only multiprocessing is truly parallelized.**

## So when to use which?
* IO bound problems: **use async if your libraries support it and if not, use threading.**
* CPU bound problems: use multi-processing.

![](../../images/1_uBLNq0ms_pDFDutRYdckAA.webp)


### Async
Async has the problem that your code needs to **be built with it and also your libraries to support it. For example, if you have a database query but your database client does not support async, it means you cannot run multiple queries at the same time losing the benefit over synchronous code.**

Also, you should not combine async with treading. Async is not thread safe.

### Threading
Threading has the problem that **if multiple threads operate on the same data, there is a risk of corruption or even crash.** For example, if one thread closes the connection that another one is currently using, there is an obvious problem. These sorts of problems are rarer with async as each block of code should return the execution only when it is safe.

You need to make sure that the functions are thread safe if they share data or objects. Use threading locks if needed.

### Multiprocessing
It might sound that multiprocessing is the best for everything based on what we have discussed so far. However, there are many significant problems that come with multiprocessing:

It is quite expensive to create processes
You cannot share memory between processes (though you can pass data via pipes)
It’s not always trivial to serialize data for processes
