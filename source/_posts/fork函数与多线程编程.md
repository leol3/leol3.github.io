---
title: fork函数与多线程编程
date: 2021-02-20 09:44:10
tags:
---

### fork函数
当程序调用fork()函数并返回成功之后，程序就将变成两个进程，调用fork()者为父进程，后来生成者为子进程。这两个进程将执行相同的程序文本， 但却各自拥有不同的栈段、数据段以及堆栈拷贝。子进程的栈、数据以及栈段开始时是父进程内存相应各部分的完全拷贝，因此它们互不影响。

### fork与线程安全
fork()函数被调用之后，子进程就相当于处于`signal handler`之中，此时就不能调用线程安全的函数（用锁机制实现安全的函数），除非函数是可重入的，而只能调用异步信号安全（async-signal-safe）的函数。fork()之后，子进程不能调用：
1. malloc()。因为malloc()在访问全局状态时会加锁。
2. 任何可能分配或释放内存的函数，包括new、map::insert()、snprintf()
3. 任何pthreads函数。你不能用pthread_cond_signal()去通知父进程，只能通过读写pipe(2)来同步。
4. printf()系列函数，因为其他线程可能恰好持有stdout/stderr的锁。
5. 除了man 7 signal中明确列出的“signal安全”函数之外的任何函数。

由于这些问题，推荐在多线程程序中调用fork()的唯一情况是：其后立即调用`exec()`函数执行另一个程序，彻底隔断子进程与父进程的关系。由新的进程覆盖掉原有的内存，使得子进程中的所有pthreads对象消失。

### pthread_atfork
对于那些必须执行fork()，而其后又无exec()紧随其后的程序来说，pthreads API提供了一种机制：fork()处理函数。利用函数`pthread_atfork()`来创建fork()处理函数。
