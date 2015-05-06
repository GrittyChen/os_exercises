# 同步互斥(lec 18) spoc 思考题


- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。

## 个人思考题

### 基本理解
 - 什么是信号量？它与软件同步方法的区别在什么地方？
 - 什么是自旋锁？它为什么无法按先来先服务方式使用资源？
 - 下面是一种P操作的实现伪码。它能按FIFO顺序进行信号量申请吗？
```
 while (s.count == 0) {  //没有可用资源时，进入挂起状态；
        调用进程进入等待队列s.queue;
        阻塞调用进程;
}
s.count--;              //有可用资源，占用该资源； 
```

> 参考回答： 它的问题是，不能按FIFO进行信号量申请。
> 它的一种出错的情况
```
一个线程A调用P原语时，由于线程B正在使用该信号量而进入阻塞状态；注意，这时value的值为0。
线程B放弃信号量的使用，线程A被唤醒而进入就绪状态，但没有立即进入运行状态；注意，这里value为1。
在线程A处于就绪状态时，处理机正在执行线程C的代码；线程C这时也正好调用P原语访问同一个信号量，并得到使用权。注意，这时value又变回0。
线程A进入运行状态后，重新检查value的值，条件不成立，又一次进入阻塞状态。
至此，线程C比线程A后调用P原语，但线程C比线程A先得到信号量。
```

### 信号量使用

 - 什么是条件同步？如何使用信号量来实现条件同步？
 - 什么是生产者-消费者问题？
 - 为什么在生产者-消费者问题中先申请互斥信息量会导致死锁？

### 管程

 - 管程的组成包括哪几部分？入口队列和条件变量等待队列的作用是什么？
 - 为什么用管程实现的生产者-消费者问题中，可以在进入管程后才判断缓冲区的状态？
 - 请描述管程条件变量的两种释放处理方式的区别是什么？条件判断中while和if是如何影响释放处理中的顺序的？

### 哲学家就餐问题

 - 哲学家就餐问题的方案2和方案3的性能有什么区别？可以进一步提高效率吗？

### 读者-写者问题

 - 在读者-写者问题的读者优先和写者优先在行为上有什么不同？
 - 在读者-写者问题的读者优先实现中优先于读者到达的写者在什么地方等待？
 
## 小组思考题

1. （spoc） 每人用python threading机制用信号量和条件变量两种手段分别实现[47个同步问题](07-2-spoc-pv-problems.md)中的一题。向勇老师的班级从前往后，陈渝老师的班级从后往前。请先理解[]python threading 机制的介绍和实例](https://github.com/chyyuu/ucore_lab/tree/master/related_info/lab7/semaphore_condition)

> 代码如下:
信号量方法:  
```
#coding=utf-8
import threading  

N = 10
mutex = threading.Semaphore(1)
Beginready = threading.Semaphore(0)
Testready = threading.Semaphore(0)
Endready = threading.Semaphore(0)

def thread_run(name):
	if(name == "teacher"):
		mutex.acquire()
		print "老师进教室!\n"
		mutex.release()
		for i in range(N):
			Beginready.acquire()
		print "开始发卷!\n"
		Testready.release()
		for i in range(N):
			Endready.acquire()
		print "老师离开!\n"
	else:
		mutex.acquire()
		print "学生进教室!\n"
		mutex.release()
		Beginready.release()
		Testready.acquire()
		Testready.release()
		print "答题\n"
		print "交卷\n"
		print "离开\n"
		Endready.release()

def main(thread_num):  
    thread_list = list();  
    # 先创建线程对象  
    for i in range(0, thread_num):
 		if(i == 0):
 			thread_name = "teacher"
 			thread_list.append(threading.Thread(target = thread_run, name = thread_name, args = (thread_name,)))
 		else:
 			thread_name = "student"
 			thread_list.append(threading.Thread(target = thread_run, name = thread_name, args = (thread_name,)))
      
    # 启动所有线程     
    for thread in thread_list:  
        thread.start()  
      
    # 主线程中等待所有子线程退出  
    for thread in thread_list:  
        thread.join()  
  
if __name__ == "__main__":  
    main(N+1)  
```
条件量方法:  
```
#coding=utf-8
import threading  

N = 10#学生数量
mutex = threading.Condition()
cond = threading.Condition()
end = threading.Condition()
Beginready = -9
Testready = 0
Endready = -9

def thread_run(name):
	global Testready
	global Beginready
	global Endready
	if(name == "teacher"):
		mutex.acquire()
		print "老师进教室!\n"
		mutex.release()
		cond.acquire()
		if(Beginready < 1):
			cond.wait()
		print "开始发卷!\n"
		Testready += 1
		cond.notify()
		cond.release()
		end.acquire()
		if(Endready != 1):
			end.wait()
		print "老师离开!\n"
		end.release()
	else:
		mutex.acquire()
		print "学生进教室!\n"
		Beginready += 1
		mutex.release()
		cond.acquire()
		cond.notifyAll()
		if(Testready != 1):
			cond.wait()
		print "答题\n"
		print "交卷\n"
		print "离开\n"
		cond.release()
		end.acquire()
		Endready += 1
		end.notify()
		end.release()

def main(thread_num):  
    thread_list = list();  
    # 先创建线程对象  
    for i in range(0, thread_num):
 		if(i == 0):
 			thread_name = "teacher"
 			thread_list.append(threading.Thread(target = thread_run, name = thread_name, args = (thread_name,)))
 		else:
 			thread_name = "student"
 			thread_list.append(threading.Thread(target = thread_run, name = thread_name, args = (thread_name,)))
      
    # 启动所有线程     
    for thread in thread_list:  
        thread.start()  
      
    # 主线程中等待所有子线程退出  
    for thread in thread_list:  
        thread.join()  
  
if __name__ == "__main__":  
    main(N+1)  
```
