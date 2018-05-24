---
title: Java多线程之synchronized、Lock、volatile
date: 2018-05-16 16:36:25
updated: 2018-05-16 16:36:25
categories: JAVA
tags: [JAVA,多线程]
description: synchronized、Lock、volatile 特点以及区别
copyright: true
comments: true
---
<!--more-->
## synchronized
> Java的关键字，是Java的内置特性，在JVM层面实现了对临界资源的同步互斥访问，通过对对象的头文件来操作，从而达到加锁和释放锁的目的

* synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生
* 不能响应中断
* 同一时刻不管是读还是写都只能有一个线程对共享资源操作，其他线程只能等待，性能不高

> synchronized是Java中的关键字，是一种同步锁：

1. 无论synchronized关键字加在方法上还是对象上，如果它作用的对象是非静态的，则它取得的锁是对象；如果synchronized作用的对象是一个静态方法或一个类，则它取得的锁对应的是类，该类所有的对象同一把锁。 
2. 每个对象只有一个锁（lock）与之相关联，谁拿到这个锁谁就可以运行它所控制的那段代码。 
3. 实现同步是要很大的系统开销作为代价的，甚至可能造成死锁，所以尽量避免无谓的同步控制

## lock

* Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现
* Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁
* Lock可以让等待锁的线程响应中断，synchronized不可以
* 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到
* Lock可以提高多个线程进行读操作的效率
```
public interface Lock {
    /**
     * 获取锁，如果锁被其他线程获取，则进行等待
     */
    void lock();

    /**
     * 当通过这个方法去获取锁时，如果线程正在等待获取锁，则这个线程能够响应中断，
     * 即中断线程的等待状态。也就是说，
     * 当两个线程同时通过lock.lockInterruptibly()想获取某个锁时，
     * 假若此时线程A获取到了锁，而线程B只有在等待，
     * 那么对线程B调用threadB.interrupt()方法能够中断线程B的等待过程。
     *
     * @throws InterruptedException
     */
    void lockInterruptibly() throws InterruptedException;

    /**
     * tryLock()方法是有返回值的，它表示用来尝试获取锁，如果获取成
     * 功，则返回true，如果获取失败（即锁已被其他线程获取），则返回
     * false，也就说这个方法无论如何都会立即返回。在拿不到锁时不会一直在那等待。
     */
    boolean tryLock();

    /**
     * tryLock(long time, TimeUnit unit)方法和tryLock()方法是类似的，
     * 只不过区别在于这个方法在拿不到锁时会等待一定的时间，
     * 在时间期限之内如果还拿不到锁，就返回false。
     * 如果如果一开始拿到锁或者在等待期间内拿到了锁，则返回true。
     *
     * @param time
     * @param unit
     * @return
     * @throws InterruptedException
     */
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

    void unlock(); //释放锁

    Condition newCondition();
}
```
## volatile
* 可见性：保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的
* 有序性：禁止进行指令重排序

加入volatile关键字时，会多出一个lock前缀指令，lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：
1. 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成
2. 它会强制将对缓存的修改操作立即写入主存
3. 如果是写操作，它会导致其他CPU中对应的缓存行无效

## lock和synchronized的区别
1. Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现；
2. synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；
3. Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；
4. 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。
5. Lock可以提高多个线程进行读操作的效率。
在性能上来说，如果竞争资源不激烈，两者的性能是差不多的，而当竞争资源非常激烈时（即有大量线程同时竞争），此时Lock的性能要远远优于synchronized

## volatile和synchronized区别
1. volatile本质是在告诉jvm当前变量在寄存器中的值是不确定的,需要从主存中读取,synchronized则是锁定当前变量,只有当前线程可以访问该变量,其他线程被阻塞住.
2. volatile仅能使用在变量级别,synchronized则可以使用在变量,方法.
3. volatile仅能实现变量的修改可见性,而synchronized则可以保证变量的修改可见性和原子性.
> 《Java编程思想》上说，定义long或double变量时，如果使用volatile关键字，就会获得（简单的赋值与返回操作）原子性。 
4. volatile不会造成线程的阻塞,而synchronized可能会造成线程的阻塞.
5. 当一个域的值依赖于它之前的值时，volatile就无法工作了，如n=n+1,n++等。如果某个域的值受到其他域的值的限制，那么volatile也无法工作，如Range类的lower和upper边界，必须遵循lower<=upper的限制。
6. 使用volatile而不是synchronized的唯一安全的情况是类中只有一个可变的域



## 锁

### 可重入锁
如果锁具备可重入性，则称作为可重入锁。像**synchronized和ReentrantLock（唯一实现了Lock接口的类）都是可重入锁**，可重入性在我看来实际上表明了锁的分配机制：基于线程的分配，而不是基于方法调用的分配。举个简单的例子，当一个线程执行到某个synchronized方法时，比如说method1，而在method1中会调用另外一个synchronized方法method2，此时线程不必重新去申请锁，而是可以直接执行方法method2

###  可中断锁

**在Java中，synchronized就不是可中断锁，而Lock是可中断锁**
如果某一线程A正在执行锁中的代码，另一线程B正在等待获取该锁，可能由于等待时间过长，线程B不想等待了，想先处理其他事情，我们可以让它中断自己或者在别的线程中中断它，这种就是可中断锁

### 公平锁
公平锁即尽量以请求锁的顺序来获取锁。比如同是有多个线程在等待一个锁，当这个锁被释放时，等待时间最久的线程（最先请求的线程）会获得该所，这种就是公平锁。
非公平锁即无法保证锁的获取是按照请求锁的顺序进行的。这样就可能导致某个或者一些线程永远获取不到锁。
**在Java中，synchronized就是非公平锁，它无法保证等待的线程获取锁的顺序**。
而对于ReentrantLock和ReentrantReadWriteLock，它默认情况下是非公平锁，但是可以设置为公平锁

参考：[java中volatile、synchronized和lock解析](https://blog.csdn.net/ztchun/article/details/60778950)、[Java并发编程：Lock](http://www.cnblogs.com/dolphin0520/p/3923167.html)