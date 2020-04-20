

# Python Notes

python一些笔记和面试题

[toc]


#### Git

Permission demied, please try again.   没有设置 ssh key

在系统  cd ~/.ssh/

两文件 id_rsa 私钥   id_rsa.pub 公钥

在gitlab, 或者其它git仓库, 设置中填写ssh key (公钥内容)

如果没有.ssh, 则 mkdir ~/.ssh

cd .ssh

在.ssh路径下配置全局name和email

git config --global user.nbame  "XXX"

git config --global user.email "XXX@XXX"

输入 ssh-keygen -t rsa -C "XXX@XXX"

生成 id_rsa和id_rsa.pub

```python
git clone -b develop XXX   # develop 分支名 XXX 地址
git init # 初始化为git仓库
git add <file> #将工作区指定文件放入暂存区
git status  # 查看工作区和暂存区的状态
git diff  # 查看修改文件
git commit -m '提交原因' # 将暂存区的内容添加到仓库
git push  # 添加到远程仓库
git log . # 查看提交日志
git checkout --<file>  # 将暂存区文件恢复到工作区
git reset --hard HEAD^ # 回到上一个版本
git reset --hard <ID> # 回到指定版本
git reflog  # 查看历史和未来版本
git pull  # 将服务器更新同步到本地
git fetch # 从远程获取最新到本地,不会自动merge
git fetch orgin master #将远程仓库的master分支 下载到本地当前branch中
git log -p master ..orgin/master # 比较本地master分支和origin/master分支的差别
git merge orign/master  # 进行合并
git fetch origin master : tmp 从远程仓库master分支获取最新在本地建立tmp分支
git diff tmp # 当前分支与tmp对比
git merge tmp # 合并tmp分支到当前分支

git branch 分支名  # 创建分支
git btanch -a # 查看所有分支
git branch -d # 删除分支
           -D # 强删
git commit --amend # 重新提交

git commit -m 'initial commit'
git add forgotten -file
git commit --amend

git reset  # 产生一个新的commit  安全
git revert

svn # 集中式
git # 分布式
pyc # 编译文件

with # 省掉关闭

Scrum
Product Backlog  # 头脑风暴
Sprint Backlog # 迭代周期
Daily metting # 站立会议
burn down # 燃烬
review meeting
release

```



####  系统体系架构图

*TODO*

#### 进程、线程、协程

1. 什么是进程？

   ​    一个正在运行的应用程序就是一个进程。

   ​    系统会给每个进程分配一个独立的内存区域，用来保存程序运行过程中产生的数据，当进程结束的时候，这个内存区域会自动销毁。

   ​    进程之间内存是不共享的，进程间通信有两种主要形式，队列（queue）和管道（pipe）。

   ​    

   ​    python中有GIL来防止多个线程同时执行本地字节码，这个锁对CPython是必须的，因为CPython的内存管理并不是线程安全的，因为GIL的存在多线程并不能发挥CPU的多核特性。

   ​    多进程可以有效的解决GIL的问题，实现多进程主要的类是Process。

   ​    对于计算密集型任务应该考虑使用多进程。

2. 什么是线程？

   ​    进程想要执行业务，就必须要有线程。每个进程默认都有一个线程,这个线程叫主线程;其它的线程叫子线程,默认都是在主线程中执行的。
   
   ​    一个线程中执行多个任务，任务是串行执行的。（一个一个按顺序执行）
   
   ​    一个进程中如果有多个线程，多线程执行不同任务的时候是并行。（同时执行）
   
   ​    进程内所有线程都共享地址空间，但每个线程有自己的栈空间。
   
3. 多线程和多进程的比较

   以下情况需要使用多线程：

   - 程序需要维护许多共享的状态（尤其是可变状态），python中的列表、字典、集合都是线程安全的，所以使用线程而不是进程维护共享状态的代价相对较小。

   - 程序会花费大量时间在I/O操作上，没有太多并行计算的需求且不需占用太多的内存。

   
   以下情况需要使用多进程:
   
   - 程序执行计算密集型任务（如：字节码操作，数据处理，科学计算）。
   - 程序的输入可以并行成块，并且可以将运算结果合并。
   - 程序在内存使用方面没有任何限制且不强依赖于I/O操作。（如：读写文件，套接字）。
   
4. 什么是协程？

   ​    协程又称微线程，纤程，协程是一种用户态的轻量级线程。即，协程由用户自己控制调度。

   协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其它地方，在切换回来的时候，恢复先前保存的寄存器上下文和栈。

   ​    因此，协程能保留上次调用时的状态，即所有局部态的特定组合，每次过程重入时，就相当于进入上一次调用状态。

   ​    协程本质上个单进程，协程相对于多进程说，无需线程上下文切换的开销，无需原子操作锁定及同步的开销，编程模型也简单。（并发却不并行）

   优点：

   - 协程的切换开销更小，属于程序级别的切换，更加轻量级。
   - 单线程内就可以实现并发的效果，最大限度利用CPU。

   缺点：

   - 协程的本质是单线程下，无法利用多核，可以是一个程序开启多个进程，每个进程内开启多个线程，每个线程内开启协程。
   - 协程指的是单个线程，因而一旦协程出现阻塞，将会阻塞整个线程。

   Greenlet

   greenlet 是一个用C实现的协程模块，相比与Python自带的yield，它可以使你在任意函数之间随意切换，而不需把这个函数先声明为generator。

#### Pyhton中怎样使用多线程？

python通过内置的threading模块来提供多线程相关技术;
其中有一个Thread类,这个类的对象就是线程对象.
直接创建线程类的对象

线程对象 = Thread(target = 函数, args = 参数)

函数  -- function 类型的变量;(这个 函数的函数体会在子线程中执行)

参数  -- 元组; 参数会传给target对应的函数

让线程开始执行任务

线程对象.start()

```python
def download(film):
    print(f'开始下载: {film}')
    
t1 = threading.Thread(target = download, args = ('小猪佩奇',))
t2 = threading.Thread(target = download, args = ('霸王别姬',))

t1.start()
t2.start()
```

某个线程出现异常,是线程直接结束,进程不一定结束。

所有的线程都结束，进程才会结束。

```python
import threading
import time


def f():
    print('start f')
    time.sleep(2)
    print('end f')
    
def main():
    thread = threading.Thread(target = f)
    thread.start()

if __name__ == '__main__':
    main()
```

```python
import threading
import random


result = []


def f():
    result.append(sum([random.randint(1,100) for i in range(10000)]))
    print(result)

    
threads = [threading.Thread(target=f) for _ in range(8)]
for thread in threads:
    thread.start()
for thread in threads:
    thread.join()
print(result)
```

多进程

```python

import time
from multiprocessing import Process


def f():
    print('enter f')
    time.sleep(2)
    print('exit f')


def main():
    p = Process(target=f)
    p.start()
    p.join()
    print('exit main')


if __name__ == "__main__":
    main()
```

```python
# TODO
# 此代码有问题,待调试
import multiprocessing
import random


def f(n):
    print('Hello, word!')
    result = sum([random.randint(1,100) for i in range(10000)])
    print('got result %d', n)
    return result


pool = multiprocessing.Pool(processes = 8)

result = pool.map(f,range(10))
print(result)

```

```python
from multprocessingfrom multiprocessing import Process
import random


result = []


def f():
    result.append(sum([random.randint(1,100) for i in range(100000)]))
    print(result)


def main():
    processes = [Process(target=f) for _ in range(8)]

    for process in processes:
        process.start()
    for process in processes:
        process.join()


if __name__ == '__main__':
    main()
```

进程,管道

```python
from multiprocessing import Process, Queue

def f(q):
    print ('enter f')
    q.put('Roctey')
    print('exit f')


def main():
    q = Queue()
    q.put('carmark')
    process = Process(target=f, args=(q,))
    process.start()
    process.join()
    while (not q.empty()):
        print(q.get_nowait())


if __name__ == '__main__':
    main()
```

进程,锁

```python
import random
import time
from multiprocessing import Process, Lock


def f(i, lock):
    lock.acquire() # 获取锁
    time.sleep(random.randint(1,3))
    print(i)
    lock.release()  # 释放锁


def main():
    lock = Lock()
    processes = [Process(target=f, args=(i, lock)) for i in range(20)]
    for process in processes:
        process.start()


if __name__ == '__main__':
    main()
```



#### 装饰器

  装饰器本质上是一个pyhton函数，它可以让其它函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。它经常用于有切面需求的场景，比如：插入日志，性能测试，事务处理，缓存，权限校验等场景。

  装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。

  概括的讲，装饰器的作用就是为存在的对象添加额外的功能。







#### Liunx

##### 以下符号在LINUX下代表什么文件：p , l, c, d, s

d：表示是一个目录（directory），事实上在ext2fs中，目录是一个特殊的文件。
－：表示这是一个普通的文件。
l: 表示这是一个符号链接(symbol link)文件，实际上它指向另一个文件。
b、c：分别表示区块(block)设备和字符（character）设备，是特殊类型的文件。
s、p：这些文件关系到系统的数据结构和管道(pipe)，通常很少见到。

##### 底下的目录与主要放置什么数据:

- /etc/: 几乎系统的所有配置文件案均在此,尤其 passwd,shadow
- /boot: 开机配置文件,也是预设摆放核心 vmlinuz的地方
- /usr/bin,/bin: 一般执行档摆放的地方
- /usr/sbin,/sbin: 系统管理员常用指令集
- /dev: 摆放所有系统装置文件的目录
- /var/log: 摆放系统注册表文件的地方
- /run: CentOS 7 以后才有,将经常变动的项目 (每次开机都不同,如程序的PID)移动到内存暂存,所以/run并不占实际磁盘容量


