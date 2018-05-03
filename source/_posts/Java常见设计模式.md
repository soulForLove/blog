---
title: Java常见设计模式
date: 2018-04-16 13:30:03
updated: 2018-04-16 13:30:03
categories: [JAVA]
tags: [JAVA,设计模式]
description: 单例/工厂/代理/适配...
copyright: true
comments: true
---
<!--more-->
## 单例模式
> 保证一个类仅有一个实例，并提供一个访问它的全局访问点

### 懒汉式
```
public class Singleton {  
    /* 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载 */  
    private static Singleton instance = null;  
  
    /* 私有构造方法，防止被实例化 */  
    private Singleton() {}  
  
    /* 1:懒汉式，静态工程方法，创建实例 */  
    public static Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}  
```
调用：
Singleton.getInstance().method(); 
**优点**：延迟加载（需要的时候才去加载）,适合单线程操作
**缺点**： 线程不安全，在多线程中很容易出现不同步的情况，如在数据库对象进行的频繁读写操作时。

### 饿汉模式
> 在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快


```
public class EagerSingleton {
    //饿汉单例模式
    //在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快
    private static EagerSingleton instance = new EagerSingleton();//静态私有成员，已初始化

    private EagerSingleton() 
    {
        //私有构造函数
    }
    public static EagerSingleton getInstance()    //静态，不用同步（类加载时已初始化，不会有多线程的问题）
    {
        return instance;
    }
}
```

### 双重线程检查模式
```
public class SingletonInner {  
    private static volatile SingletonInner sInst = null;  // <<< 这里添加了 volatile  
  
    /** 
     * 私有的构造函数 
     */  
    private SingletonInner() {}  
  
    public static SingletonInner getInstance() {  
        SingletonInner inst = sInst;  // <<< 在这里创建临时变量
        if (inst == null) {
            synchronized (SingletonInner.class) {
                inst = sInst;
                if (inst == null) {
                    inst = new SingletonInner();
                    sInst = inst;
                }
            }
        }
        return inst;  // <<< 注意这里返回的是临时变量
    }
    protected void method() {  
        System.out.println("SingletonInner");  
    }  
}  
```
**volatile**：
**可见性**（volatile 保证了新值能立即同步到主内存，以及每次使用前立即从主内存刷新）
**防止指令重排序**（赋值后多执行了一个“load addl $0x0, (%esp)”操作，这个操作相当于一个内存屏障）
调用：
Singleton.getInstance().method();  
**优点**：延迟加载，线程安全
**缺点**：写法复杂，不简洁

### 内部类模式
```
public class SingletonInner {  
    /** 
     * 内部类实现单例模式 
     * 延迟加载，减少内存开销   
     */  
    private static class SingletonHolder {  
        private static SingletonInner instance = new SingletonInner();  
    }  
  
    /** 
     * 私有的构造函数 
     */  
    private SingletonInner() {}  
  
    public static SingletonInner getInstance() {  
        return SingletonHolder.instance;  
    }  
  
    protected void method() {  
        System.out.println("SingletonInner");  
    }  
} 
```
调用：
Singleton.getInstance().method();  
**优点**：延迟加载，线程安全（java中class加载时互斥的），也减少了内存消耗，推荐使用内部类方式。