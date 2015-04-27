# lab5 spoc 思考题

- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。


## 个人思考题

### 总体介绍

 - ucore中的调度点在哪里，完成了啥事？
 - 进程控制块中与调度相关的字段有哪些？

> need_schedule, wait, exit, run

 - ucore的就绪队列数据结构在哪定义？在哪进行修改？
 - ucore的等待队列数据结构在哪定义？在哪进行修改？

### 调度算法支撑框架

 - 调度算法支撑框架中的各个函数指针的功能是啥？会被谁在何种情况下调用？
 - 调度函数schedule()的调用函数分析，可以了解进程调度的原因。请分析ucore中所有可能的调度位置，并说明可能的调用原因。
 
### 时间片轮转调度算法

 - 时间片轮转调度算法是如何基于调度算法支撑框架实现的？
 - 时钟中断如何调用RR_proc_tick()的？

### stride调度算法

 - stride调度算法的思路？ 
 - stride算法的特征是什么？
 - stride调度算法是如何避免stride溢出问题的？
 - 无符号数的有符号比较会产生什么效果？
 - 什么是斜堆(skew heap)?

## 小组练习与思考题

### (1)(spoc) 理解调度算法支撑框架的执行过程

即在ucore运行过程中通过`cprintf`函数来完整地展现出来多个进程在调度算法和框架的支撑下，在相关调度点如何动态调度和执行的细节。(约全面细致约好)

请完成如下练习，完成代码填写，并形成spoc练习报告
> 需写练习报告和简单编码，完成后放到git server 对应的git repo中

### 练习用的[lab6 spoc exercise project source code](https://github.com/chyyuu/ucore_lab/tree/master/labcodes_answer/lab6_result)

> schedule has been run because current->neeed_resched is !  
process 1 be picked, its lab6_stride is 2147483647  
process 1 be dequeued  
current's pid: 0  
 next's pid: 1  
process 2 be enqueued  
schedule has been run, because do_wait()!  
process 2 be picked, its lab6_stride is 2147483647  
process 2 be dequeued  
current's pid: 1  
 next's pid: 2  
kernel_execve: pid = 2, name = "forktest".  
process 3 be enqueued  
process 4 be enqueued  
process 5 be enqueued  
process 6 be enqueued  
process 7 be enqueued  
process 8 be enqueued  
process 9 be enqueued  
process 10 be enqueued  
process 11 be enqueued  
process 12 be enqueued  
process 13 be enqueued  
process 14 be enqueued  
process 15 be enqueued  
process 16 be enqueued  
process 17 be enqueued  
process 18 be enqueued  
process 19 be enqueued  
process 20 be enqueued  
process 21 be enqueued  
process 22 be enqueued  
process 23 be enqueued  
process 24 be enqueued  
process 25 be enqueued  
process 26 be enqueued   
process 28 be enqueued  
process 29 be enqueued  
process 30 be enqueued  
process 31 be enqueued  
process 32 be enqueued  
process 33 be enqueued  
process 34 be enqueued  
schedule has been run, because do_wait()!  
process 34 be picked, its lab6_stride is 2147483647  
process 34 be dequeued  
current's pid: 2  
 next's pid: 34  
I am child 31  
process 2 be enqueued  
schedule has been run, because do_exit()!  
process 33 be picked, its lab6_stride is 2147483647  
process 33 be dequeued  
current's pid: 34  
 next's pid: 33  
I am child 30  
schedule has been run, because do_exit()!  
process 32 be picked, its lab6_stride is 2147483647   
process 32 be dequeued  
current's pid: 33  
 next's pid: 32  
I am child 29  
schedule has been run, because do_exit()!  
process 31 be picked, its lab6_stride is 2147483647  
process 31 be dequeued  
current's pid: 32   
 next's pid: 31  
I am child 28  
schedule has been run, because do_exit()!  
process 30 be picked, its lab6_stride is 2147483647  
process 30 be dequeued  
current's pid: 31  
 next's pid: 30  
I am child 27  
schedule has been run, because do_exit()!   
process 29 be picked, its lab6_stride is 2147483647  
process 29 be dequeued  
current's pid: 30  
 next's pid: 29  
I am child 26  
schedule has been run, because do_exit()!  
process 28 be picked, its lab6_stride is 2147483647  
process 28 be dequeued  
current's pid: 29  
 next's pid: 28  
I am child 25  
schedule has been run, because do_exit()!  
process 27 be picked, its lab6_stride is 2147483647  
process 27 be dequeued  
current's pid: 28  
 next's pid: 27  
I am child 24  
schedule has been run, because do_exit()!  
process 26 be picked, its lab6_stride is 2147483647  
process 26 be dequeued  
current's pid: 27  
 next's pid: 26   
I am child 23  
schedule has been run, because do_exit()!  
process 25 be picked, its lab6_stride is 2147483647  
process 25 be dequeued  
current's pid: 26  
 next's pid: 25  
I am child 22  
schedule has been run, because do_exit()!  
process 24 be picked, its lab6_stride is 2147483647  
process 24 be dequeued   
current's pid: 25  
 next's pid: 24  
I am child 21  
schedule has been run, because do_exit()!  
process 23 be picked, its lab6_stride is 2147483647  
process 23 be dequeued  
current's pid: 24  
 next's pid: 23  
I am child 20  
schedule has been run, because do_exit()!   
process 22 be picked, its lab6_stride is 2147483647 
process 22 be dequeued  
current's pid: 23  
 next's pid: 22  
I am child 19  
schedule has been run, because do_exit()!  
process 21 be picked, its lab6_stride is 2147483647  
process 21 be dequeued  
current's pid: 22   
 next's pid: 21  
I am child 18   
schedule has been run, because do_exit()!  
process 20 be picked, its lab6_stride is 2147483647  
process 20 be dequeued  
current's pid: 21  
 next's pid: 20  
I am child 17  
schedule has been run, because do_exit()!  
process 19 be picked, its lab6_stride is 2147483647   
process 19 be dequeued  
current's pid: 20  
 next's pid: 19  
I am child 16  
schedule has been run, because do_exit()!  
process 18 be picked, its lab6_stride is 2147483647  
process 18 be dequeued  
current's pid: 19  
 next's pid: 18  
I am child 15  
schedule has been run, because do_exit()!  
process 17 be picked, its lab6_stride is 2147483647  
process 17 be dequeued  
current's pid: 18  
 next's pid: 17  
I am child 14  
schedule has been run, because do_exit()!  
process 16 be picked, its lab6_stride is 2147483647  
process 16 be dequeued  
current's pid: 17  
 next's pid: 16  
I am child 13  
schedule has been run, because do_exit()!  
process 15 be picked, its lab6_stride is 2147483647  
process 15 be dequeued  
current's pid: 16  
 next's pid: 15  
I am child 12  
schedule has been run, because do_exit()!  
process 14 be picked, its lab6_stride is 2147483647  
process 14 be dequeued  
current's pid: 15  
 next's pid: 14  
I am child 11  
schedule has been run, because do_exit()!  
process 13 be picked, its lab6_stride is 2147483647  
process 13 be dequeued  
current's pid: 14  
 next's pid: 13  
I am child 10  
schedule has been run, because do_exit()!  
process 12 be picked, its lab6_stride is 2147483647  
process 12 be dequeued  
current's pid: 13  
 next's pid: 12  
I am child 9  
schedule has been run, because do_exit()!  
process 11 be picked, its lab6_stride is 2147483647  
process 11 be dequeued  
current's pid: 12  
 next's pid: 11  
I am child 8  
schedule has been run, because do_exit()!  
process 10 be picked, its lab6_stride is 2147483647  
process 10 be dequeued  
current's pid: 11  
 next's pid: 10  
I am child 7  
schedule has been run, because do_exit()!  
process 9 be picked, its lab6_stride is 2147483647  
process 9 be dequeued  
current's pid: 10  
 next's pid: 9  
I am child 6  
schedule has been run, because do_exit()!  
process 8 be picked, its lab6_stride is 2147483647  
process 8 be dequeued  
current's pid: 9  
 next's pid: 8  
I am child 5  
schedule has been run, because do_exit()!  
process 7 be picked, its lab6_stride is 2147483647  
process 7 be dequeued  
current's pid: 8  
 next's pid: 7   
I am child 4  
schedule has been run, because do_exit()!  
process 6 be picked, its lab6_stride is 2147483647  
process 6 be dequeued  
current's pid: 7  
 next's pid: 6  
I am child 3  
schedule has been run, because do_exit()!  
process 5 be picked, its lab6_stride is 2147483647  
process 5 be dequeued  
current's pid: 6  
 next's pid: 5  
I am child 2  
schedule has been run, because do_exit()!  
process 4 be picked, its lab6_stride is 2147483647  
process 4 be dequeued  
current's pid: 5  
 next's pid: 4  
I am child 1  
schedule has been run, because do_exit()!  
process 3 be picked, its lab6_stride is 2147483647  
process 3 be dequeued  
current's pid: 4  
 next's pid: 3  
I am child 0  
schedule has been run, because do_exit()!  
process 2 be picked, its lab6_stride is -2  
process 2 be dequeued  
current's pid: 3  
 next's pid: 2  
forktest pass.  
process 1 be enqueued  
schedule has been run, because do_exit()!  
process 1 be picked, its lab6_stride is -2  
process 1 be dequeued  
current's pid: 2  
 next's pid: 1  
schedule has been run, because do_wait()!  
process 1 be enqueued  
process 1 be picked, its lab6_stride is 2147483645  
process 1 be dequeued  
all user-mode processes have quit.  


