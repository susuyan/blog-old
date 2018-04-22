
---
layout: post
title: "多线程--第一篇"
date: 2015-03-16 19:07:11.000000000 +09:00
tags: 能工巧匠集
---

在学习多线程时,先简单的回忆下进程的一个概念

### 什么是进程

*  狭义定义: 进程就是一段程序的执行过程 
*  广义定义: 进程是一个具有一定独立功能的程序关于某个数据集合的一次运行活动.它是操作系统动态执行的基本单元,在传统的操作系统中,进程既是基本的分配单元,也是执行单元.

### 进一步理解

 (不要理解太深,有时间得回看一下之前学习的操作系统)
*  进程是一个实体,每一个进程都有它自己的地址空间,一般情况下,包括文本区域(写的以一些代码),数据区域(各种变量)和堆栈.
*  进程是一个"执行中的程序"

### 什么是线程

回忆完进程后,就可以引出线程了  
<!-- more -->
* 进程作为分配资源的基本单位,而把线程作为独立运行和独立调度的基本单位,由于线程比金车功能更小,基本上不拥有系统资源,故对它的调度所付出的开销就会小得多,能高效的提高系统内多个程序间并发执行的程度 
* 一个线程就是运行在进程上下文的一个逻辑流。现代操作系统允许我们编写一个进程里同时运行多个线程的程序。线程由内核调度。每个进程开始生命周期时都是单一线程，这个线程称为主线程，在某一时刻，主线程创建一个对等线程。从这个时间点开始，两个线程就并发运行。最后，因为主线程执行一个慢速系统调用，如read或者sleep，控制就会通过上下文切换传递到对等线程。在控制传递回主线程前，对等线程会执行一段时间

### 为什么要用多线程

在 iOS 应用中,一启动后就会创建一个主线程(也可以说为 UI线程),我们的各种View的加载和更新,都是在主线程里.有这样的一个应用场景(我们的酷炫的界面,有的时候需要进行多得网络操作加载资源,还有下载音乐的...这些都是很耗时的,如果只是在一个线程里当然也可以做,你能想象到那种应用操作的流畅度,各种卡顿)所以,更多的时候,是要把费时的操作放在另一个对等的线程.

### 长用的多线程开发方式

1. NSThread
2. NSOperation
3. GCD
 
它们的抽象程度由低到高,越高的使用起来越简单.

### NSThread
#### 显示调用 NSthread 类
* 类方法  


		[NSThread detachNewThreadSelector:@selector(doSomething:)		toTarget:self withObject:@"hi"];
	
* 实例方法

		[self performSelectorOnMainThread:@selector(doSomething:)		withObject:@"hi" waitUntilDone:YES];

#### 隐式调用

* 开启后台线程  
		
		[self performSelectorInBackground:@selector(doSomething:)		withObject:@"hi"];

* 在主线程中运行

		[self performSelectorOnMainThread:@selector(doSomething:)		withObject:@"hi" waitUntilDone:YES];


* 在指定线程中执行,但线程必须具备run loop

		[self performSelector::@selector(doSomething:) 		onThread:thread    withObject:@"hi" waitUntilDone:YES];

#### 常见 NSThread 的方法

	+ (NSThread *)currentThread; //获得当前线程	+ (void)sleepForTimeInterval:(NSTimeInterval)ti; //线程休眠	+ (NSThread *)mainThread; //主线程	- (BOOL)isMainThread;	+ (BOOL)isMainThread; //当前线程是否是主线程	- (BOOL)isExecuting; //线程是否正在运行	- (BOOL)isFinished; //线程是否结束

