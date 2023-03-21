---
title: 'Python 多线程'
date: 2023-3-21 00:11:41
tags:
  - python
  - thread
categories: python
mathjax: true


---





原因：



## 2.1 全局解释器锁（GIL）：

Python 代码的执行是由 Python 虚拟机(又名解释器主循环)进行控制的，python在设计的时候考虑的是在主循环中同时只能有一个控制线程在执行，就像单核 CPU系统中的多进程一样。尽管 Python 解释器中可以运行多个线程，但是在任意给定时刻只有一个线程会被解释器执行。

对 Python 虚拟机的访问是由全局解释器锁(GIL)控制的。这个锁就是用来保证同时只能有一个线程运行的。在多线程环境中，Python 虚拟机将按照下面所述的方式执行：

​	1.设置 GIL。 
​	2.切换进一个线程去运行。 
​	3.执行下面操作之一。
​		a.指定数量的字节码指令。
​		b.线程主动让出控制权(可以调用 time.sleep(0)来完成)。 
​	4.把线程设置回睡眠状态(切换出线程)。
​	5.解锁 GIL。
​    6.重复上述步骤

**当调用外部代码(即，任意 C/C++扩展的内置函数)时，GIL 会保持锁定，直至函数执行结束。**

> 对于任意面向 I/O 的 Python 例程(调用了内置的操作系统 C 代码的那种)， GIL 会在 I/O 调用前被释放，以允许其他线程在 I/O 执行的时候运行。而对于那些没有太 多 I/O 操作的代码而言，更倾向于在该线程整个时间片内始终占有处理器.
> 所以**I/O 密集型的 Python 程序要比计算密集型的代码能够更好地利用多线程环境。**



GIL的存在使得Python多线程编程暂时无法充分利用多处理器的优势，这种限制也许使很多人感到沮丧，但事实上这并不意味着我们需要放弃多线程。对于只含纯Python的代码也许使用多线程并不能提高运行速率，但在以下几种情况，如**等待外部资源返回**，或者**为了提高用户体验而建立反应灵活的用户界面**，或者**多用户应用程序**中，多线程仍然是一个比较好的解决方案。

在 CPython 中，由于存在 [全局解释器锁](https://docs.python.org/zh-cn/3.10/glossary.html#term-global-interpreter-lock)，同一时刻只有一个线程可以执行 Python 代码（虽然某些性能导向的库可能会去除此限制）。 如果你想让你的应用更好地利用多核心计算机的计算资源，推荐你使用 [`multiprocessing`](https://docs.python.org/zh-cn/3.10/library/multiprocessing.html#module-multiprocessing) 或 [`concurrent.futures.ProcessPoolExecutor`](https://docs.python.org/zh-cn/3.10/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor)。 但是，如果你想要同时运行多个 I/O 密集型任务，则多线程仍然是一个合适的模型。

## 2.2 实现

> **推荐优先使用threading模块**

Python为多线程编程提供了两个非常简单明了的模块：thread和threading，另外还有Queue。

- thread 模块：提供了基本的线程和锁定支持;
- threading 模块：提供了更高级别、功能更全面的线程管理；
- Queue模块，用户可以创建一个队列数据结构，用于在多线程之间进行共享

简单来说：**thread模块提供了多线程底层支持模块，以低级原始的方式来处理和控制线程，使用起来较为复杂；而threading模块基于thread进行包装，将线程的操作对象化，在语言层面提供了丰富的特性**。

使用threading的原因：

1. threading模块对同步原语的支持更为完善和丰富。就线程的同步和互斥来说，thread模块只提供了一种锁类型thread.LockType，而threading模块中不仅有Lock指令锁、RLock可重入指令锁，还支持条件变量Condition、信号量Semaphore、BoundedSemaphore以及Event事件等。
2. threading模块在主线程和子线程交互上更为友好，threading中的join()方法能够阻塞当前上下文环境的线程，直到调用此方法的线程终止或到达指定的timeout（可选参数）。利用该方法可以方便地控制主线程和子线程以及子线程之间的执行。
3. thread模块不支持守护线程。thread模块中主线程退出的时候，所有的子线程不论是否还在工作，都会被强制结束，并且没有任何警告也没有任何退出前的清理工作

创建线程：

- 继承Thread类，重写它的run()方法
- 创建一个threading.Thread对象，在它的初始化函数（__init__()）中将可调用对象作为参数传入。**推荐优先使用threading模块**

[相关参数](https://docs.python.org/zh-cn/3.10/library/threading.html?highlight=threading#module-threading)

关于线程信息的函数：

- `threading.active_count()`：返回当前存活的Thread对象数量。
- `threading.current_thread()`：返回当前线程的Thread对象。
- `threading.enumerate()`：列表形式返回所有存活的Thread对象。
- `threading.main_thread()`：返回主Thread对象。

Thread对象的方法及属性：

- `Thread.name`：线程的名字，没有语义，可以相同名称。
- `Thread.ident`：线程标识符，非零整数。
- `Thread.Daemon`：是否为守护线程。
- `Thread.is_alive()`：是否存活。
- `Thread.start()`：开始线程活动。若多次调用抛出RuntimeError。
- `Thread.run()`：用来重载的，
- `Thread.join(timeout=None)`：等待直到线程正常或异常结束。尚未开始抛出RuntimeError
- `Thread(group=None, target=None, name=None, args=(), kwargs={}, *, deamon=None)`：构造函数。

### 让主线程等待子线程结束 join

假如要让主线程等子线程，那么可以使用Thread.join()方法。join可以让运行这条语句的主线程在此阻塞（等待），直到子线程结束，再放行。



```python
import time
from threading import Thread

def task1():
    print("开始做任务1啦")
    time.sleep(3)  # 用time.sleep模拟任务耗时
    print("任务1结束啦")

if __name__ == '__main__':
    print("这里是主线程")
    # 创建线程对象
    t1 = Thread(target=task1)
    # t1.setDaemon(True)  # 设置为守护进程，必须在start之前
    # 启动
    t1.start()
    # 阻塞
    t1.join()
    print("主线程结束了")
```



锁对象：

```python
class threading.Lock
acquire(blocking=True, timeout=- 1)
release()
#递归锁对象:
class threading.RLock
```

**RLock**的R表示Reentrant，如果用RLock，那么在同一个线程中可以对它多次acquire，同时也要用相同数目的release来释放锁。这个东西的意义在于避免**死锁**。





```python
import time
from threading import Thread

def task():
    print("开始做一个任务啦")
    time.sleep(1)  # 用time.sleep模拟任务耗时
    print("这个任务结束啦")
    
if __name__ == '__main__':
    print("这里是主线程")
    # 创建线程对象
    t1 = Thread(target=task)
    # 启动
    t1.start()
    time.sleep(0.3)
    print("主线程依然可以干别的事")
```



```python
import time
from threading import Thread

class NewThread(Thread):
    def __init__(self):
        Thread.__init__(self)  # 必须步骤
    
    def run(self):  # 入口是名字为run的方法
        print("开始做一个任务啦")
        time.sleep(1)  # 用time.sleep模拟任务耗时
        print("这个任务结束啦")
        
if __name__ == '__main__':
    print("这里是主线程")
    # 创建线程对象
    t1 = NewThread()
    # 启动
    t1.start()
    time.sleep(0.3)
    print("主线程依然可以干别的事")
```



## 使用Queue使多线程编程更安全



## 线程池 thread pool





## [`multiprocessing`](https://docs.python.org/zh-cn/3.10/library/multiprocessing.html#module-multiprocessing) --- 基于进程的并行

[`multiprocessing`](https://docs.python.org/zh-cn/3.10/library/multiprocessing.html#module-multiprocessing) 是一个支持使用与 [`threading`](https://docs.python.org/zh-cn/3.10/library/threading.html#module-threading) 模块类似的 API 来产生进程的包。 [`multiprocessing`](https://docs.python.org/zh-cn/3.10/library/multiprocessing.html#module-multiprocessing) 包同时提供了本地和远程并发操作，通过使用子进程而非线程有效地绕过了 [全局解释器锁](https://docs.python.org/zh-cn/3.10/glossary.html#term-global-interpreter-lock)。 因此，[`multiprocessing`](https://docs.python.org/zh-cn/3.10/library/multiprocessing.html#module-multiprocessing) 模块允许程序员充分利用给定机器上的多个处理器