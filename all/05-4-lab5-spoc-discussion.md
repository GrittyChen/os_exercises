# lab5 spoc 思考题

- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。


## 个人思考题

### 总体介绍

 - 第一个用户进程创建有什么特殊的？
 - 系统调用的参数传递过程？
 - getpid的返回值放在什么地方了？
 
> 

### 进程的内存布局

 - 尝试在进程运行过程中获取内核堆栈和用户堆栈的调用栈？
 - 尝试在进程运行过程中获取内核空间中各进程相同的页表项（代码段）和不同的页表项（内核堆栈）？

### 执行ELF格式的二进制代码-do_execve的实现

 - 在do_execve中进程清空父进程时，当前进程是哪一个？在什么时候开始使用新加载进程的地址空间？
 - 新加载进程的第一级页表的建立代码在哪？

### 执行ELF格式的二进制代码-load_icode的实现

 - 第一个内核线程和第一个用户进程的创建有什么不同？
 - 尝试跟踪分析新创建的用户进程的开始执行过程？

### 进程复制

 - 为什么新进程的内核堆栈可以先于进程地址空间复制进行创建？
 - 进程复制的代码在哪？复制了哪些内容？
 - 进程复制过程中有哪些修改？为什么要修改？

### 内存管理的copy-on-write机制
 - 什么是写时复制？
 - 写时复制的页表在什么时候进行复制？共享地址空间和写时复制有什么不同？

## 小组练习与思考题

### (1)(spoc) 在真实机器的u盘上启动并运行ucore lab,

请准备一个空闲u盘，然后请参考如下网址完成练习

https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-boot-with-grub2-in-udisk.md

(报告可课后完成)请理解grub multiboot spec的含义，并分析ucore_lab是如何实现符合grub multiboot spec的，并形成spoc练习报告。

### (2)(spoc) 理解用户程的生命周期。

> 需写练习报告和简单编码，完成后放到git server 对应的git repo中

### 练习用的[lab5 spoc exercise project source code](https://github.com/chyyuu/ucore_lab/tree/master/related_info/lab5/lab5-spoc-discuss)

#### 掌握知识点
1. 用户进程的启动、运行、就绪、等待、退出
2. 用户进程的管理与简单调度
3. 用户进程的上下文切换过程
4. 用户进程的特权级切换过程
5. 用户进程的创建过程并完成资源占用
6. 用户进程的退出过程并完成资源回收

> 注意，请关注：内核如何创建用户进程的？用户进程是如何在用户态开始执行的？用户态的堆栈是保存在哪里的？

阅读代码，在现有基础上再增加一个用户进程A，并通过增加cprintf函数到ucore代码中，
能够把个人思考题和上述知识点中的内容展示出来：即在ucore运行过程中通过`cprintf`函数来完整地展现出来进程A相关的动态执行和内部数据/状态变化的细节。(约全面细致约好)

请完成如下练习，完成代码填写，并形成spoc练习报告
> process 1 run!  
process 0 wait  
process 2 run!  
kernel_execve: pid = 2, name = "exit".  
user changed to kernel  
kernel changed to user!  
user changed to kernel  
Iuser changed to kernel  
 user changed to kernel  
auser changed to kernel  
muser changed to kernel  
 user changed to kernel  
tuser changed to kernel  
huser changed to kernel  
euser changed to kernel  
 user changed to kernel  
puser changed to kernel  
auser changed to kernel  
ruser changed to kernel  
euser changed to kernel  
nuser changed to kernel  
tuser changed to kernel  
.user changed to kernel  
 user changed to kernel  
Fuser changed to kernel  
ouser changed to kernel  
ruser changed to kernel  
kuser changed to kernel  
iuser changed to kernel  
nuser changed to kernel  
guser changed to kernel  
 user changed to kernel  
tuser changed to kernel  
huser changed to kernel  
euser changed to kernel  
 user changed to kernel  
cuser changed to kernel  
huser changed to kernel  
iuser changed to kernel  
luser changed to kernel  
duser changed to kernel  
.user changed to kernel  
.user changed to kernel  
.user changed to kernel  
user changed to kernel   
user changed to kernel  
Iuser changed to kernel  
 user changed to kernel  
auser changed to kernel  
muser changed to kernel  
 user changed to kernel  
puser changed to kernel  
auser changed to kernel   
ruser changed to kernel  
euser changed to kernel  
nuser changed to kernel  
tuser changed to kernel   
,user changed to kernel   
 user changed to kernel   
fuser changed to kernel  
ouser changed to kernel  
ruser changed to kernel  
kuser changed to kernel   
 user changed to kernel   
auser changed to kernel   
 user changed to kernel   
cuser changed to kernel  
huser changed to kernel   
iuser changed to kernel  
luser changed to kernel   
duser changed to kernel   
 user changed to kernel   
puser changed to kernel   
iuser changed to kernel  
duser changed to kernel  
 user changed to kernel   
3user changed to kernel   
user changed to kernel   
Iuser changed to kernel  
 user changed to kernel  
auser changed to kernel   
muser changed to kernel   
 user changed to kernel   
tuser changed to kernel   
huser changed to kernel   
euser changed to kernel   
 user changed to kernel   
puser changed to kernel   
auser changed to kernel   
ruser changed to kernel   
euser changed to kernel   
nuser changed to kernel   
tuser changed to kernel   
,user changed to kernel   
 user changed to kernel   
wuser changed to kernel   
auser changed to kernel   
iuser changed to kernel  
tuser changed to kernel  
iuser changed to kernel   
nuser changed to kernel   
guser changed to kernel   
 user changed to kernel   
nuser changed to kernel   
ouser changed to kernel   
wuser changed to kernel   
.user changed to kernel   
.user changed to kernel   
user changed to kernel   
process 3 wait  
process 3 run!  
user changed to kernel  
Iuser changed to kernel  
 user changed to kernel  
auser changed to kernel  
muser changed to kernel   
 user changed to kernel   
tuser changed to kernel   
huser changed to kernel   
euser changed to kernel   
 user changed to kernel   
cuser changed to kernel   
huser changed to kernel   
iuser changed to kernel   
luser changed to kernel   
duser changed to kernel   
.user changed to kernel   
user changed to kernel   
procee 3 yield!  
user changed to kernel  
procee 3 yield!  
user changed to kernel  
procee 3 yield!  
user changed to kernel  
procee 3 yield!  
user changed to kernel  
procee 3 yield!  
user changed to kernel  
procee 3 yield!  
user changed to kernel   
procee 3 yield!  
user changed to kernel  
process 3 exit!  
process 2 run!  
user changed to kernel  
process 3 wait  
user changed to kernel  
process 0 wait  
user changed to kernel  
wuser changed to kernel  
auser changed to kernel  
iuser changed to kernel   
tuser changed to kernel   
puser changed to kernel   
iuser changed to kernel  
duser changed to kernel  
 user changed to kernel   
3user changed to kernel   
 user changed to kernel   
ouser changed to kernel   
kuser changed to kernel   
.user changed to kernel  
user changed to kernel   
euser changed to kernel   
xuser changed to kernel   
iuser changed to kernel  
tuser changed to kernel  
 user changed to kernel   
puser changed to kernel   
auser changed to kernel  
suser changed to kernel  
suser changed to kernel   
.user changed to kernel   
user changed to kernel    
process 2 exit!   
process 1 run!  
process 0 wait   
all user-mode processes have quit.   
init check memory pass.   
process 1 exit!   
kernel panic at kern/process/proc.c:456:  
    initproc exit.  
Welcome to the kernel debug monitor!!  
Type 'help' for a list of commands.  
