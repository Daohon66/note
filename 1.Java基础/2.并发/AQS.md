**AQS简介：**

AQS全称AbstractQueuedSynchronizer

AQS是一个用来构建锁和同步器的框架，ReentrantLock就是基于AQS。

实现可重入锁

**AQS原理：**

AQS的核心思想是，空闲的共享资源被请求时，会将当前请求资源的线程设置为有效的工作线程，并将共享资源设置为锁定状态。如果共享资源被占用，就需要一套线程阻塞等待以及被唤醒时锁分配的机制，AQS使用CLR队列锁来实现了这个机制，暂时获取不到锁的线程将加入队列中。

AQS使用一个int成员变量来表示同步状态，通过内置的FIFO队列来获取资源线程的排队工作。AQS使用CAS对该同步状态进行原子操作实现对值的修改。

**AQS对资源的共享方式**

- Exclusive（独占）：只有一个线程能够执行，如ReentrantLock。可以分为公平锁和非公平锁。 

公平锁：排队拿锁

非公平锁：可以插队拿锁

- Share（共享）：多个线程可同时执行，比如CountDownLatch。

**AQS底层使用模版方法模式**

同步器的设计基于模版模式，自定义同步器的步骤如下：

1. 使用者继承AbstractQueuedSynchronizer并重写指定方法。
2. 将AQS组合在自定义同步组件的实现中，并调用模版方法，这些模版方法会调用使用者重写的方法。

AQS提供的模版方法

```java
isHeldExclusively()//该线程是否正在独占资源。只有用到condition才需要去实现它。
tryAcquire(int)//独占方式。尝试获取资源，成功则返回true，失败则返回false。
tryRelease(int)//独占方式。尝试释放资源，成功则返回true，失败则返回false。
tryAcquireShared(int)//共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
tryReleaseShared(int)//共享方式。尝试释放资源，成功则返回true，失败则返回false。
```

默认情况下，每个方法都抛出UnsupportedOperationException。这些方法的实现必须是内部线程安全的，并且通常应该简短而不是阻塞。

> 以 ReentrantLock 为例，state 初始化为 0，表示未锁定状态。A 线程 lock()时，会调用 tryAcquire()独占该锁并将 state+1。此后，其他线程再 tryAcquire()时就会失败，直到 A 线程 unlock()到 state=0（即释放锁）为止，其它线程才有机会获取该锁。当然，释放锁之前，A 线程自己是可以重复获取此锁的（state 会累加），这就是可重入的概念。但要注意，获取多少次就要释放多么次，这样才能保证 state 是能回到零态的。
>
> 再以 CountDownLatch 以例，任务分为 N 个子线程去执行，state 也初始化为 N（注意 N 要与线程个数一致）。这 N 个子线程是并行执行的，每个子线程执行完后 countDown() 一次，state 会 **CAS**(Compare and Swap)减 1。等到所有子线程都执行完后(即 state=0)，会 unpark()主调用线程，然后主调用线程就会从 await() 函数返回，继续后余动作。