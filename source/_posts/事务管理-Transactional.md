---
title: 事务管理@Transactional
date: 2018-03-21 13:12:07
updated: 2018-03-21 13:12:07
categories: [事务]
tags: [事务]
description: 事务的作用就是为了保证用户的每一个操作都是可靠的，事务中的每一步操作都必须成功执行，只要有发生异常就回退到事务开始未进行操作的状态。
copyright: true
comments: true
---
<!--more-->
## 传播行为（生命周期）-Propagation
> 所谓事务的传播行为是指，如果在开始当前事务之前，一个事务上下文已经存在，此时有若干选项可以指定一个事务性方法的执行行为（org.springframework.transaction.annotation.Propagation）


* **REQUIRED**（默认）
方法A调用时没有事务新建一个事务，当在方法A调用另一个方法B的时候，方法B将使用同一个事务；如果方法B发生异常需要数据回滚的时候，整个事务回滚
* REQUIRED_NEW
对于方法A与B，在方法调用的时候无论是否有事务，都要开启一个新的事；如果这样方法B有异常不会导致方法A的数据回滚
* NESTED
和REQUIRED_NEW类似，但支持JDBC，不支持JPA或者Hibernate
* SUPPORTS
方法调用时有事务就用事务，没有就不用
* NOT_SUPPORTS
强制方法不在事务中执行，若有事务，在方法调用到结束阶段事务都将会被挂起
* NEVER
强制方法不在事务中执行，若有事务则抛出异常
* MANDATORY
强制方法在事务中执行，若无事务则抛出异常
指定方法：通过使用propagation属性设置
```
@Transactional(propagation = Propagation.REQUIRED)

```

## 隔离级别-Isolation
> 隔离级别是指若干个并发的事务之间的隔离程度，与我们开发时候主要相关的场景包括：脏读取、重复读、幻读；
> Isolation（隔离）决定了事务的完整性，处理在多事务对相同数据下的处理机制

* READ_UNCOMMITED
对于在A事务里修改了一条记录但没有提交事务，在B事务可以读取到修改后的记录。可导致脏读，不可重复读以及幻读
* READ_COMMITED
只有当在A事务里修改了一条记录且提交记录之后，B事务才可以读取到提交后的记录；阻止脏读，但可能导致不可重复读和幻读
* REPEATABLE_READ
不仅能实现READ_COMMITED的功能，而且还能阻止当A事务读取了一条记录，B事务将不允许修改这条记录；阻止脏读和不可重复读，但可出现幻读
* SERIALIZABLE
此级别下事务是顺序执行的，可以避免上述级别的缺陷，但开销较大
* **DEFAULT**（默认）
使用当前数据库的默认隔离界级别，如Oracle、SqlServer是READ_COMMITED；Mysql是REPEATABLE_READ
指定方法：通过使用isolation属性设置
```
@Transactional(isolation = Isolation.DEFAULT)
```
## timeout（默认TIMEOUT_DEFAULT）
timeout指定事务过期时间，默认为当前数据库的事务过期时间
## readonly（默认false）
指定当前事务是否是只读事务
## rollbackFor（默认Throwable的子类）
指定哪个或者哪些异常可以引起事务回滚
## noRollBackFor（默认Throwable的子类）
指定哪个或者哪些异常不可以引起事务回滚

```
@Transactional(propagation=Propagation.REQUIRED)           //控制事务传播。默认是Propagation.REQUIRED
@Transactional(isolation=Isolation.DEFAULT)                //控制事务隔离级别。默认跟数据库的默认隔离级别相同
@Transactional(readOnly=false)                             //控制事务可读写还是只可读。默认可读写
@Transactional(timeout=30)                                 //控制事务的超时时间，单位秒。默认跟数据库的事务控制系统相同，又说是30秒
@Transactional(rollbackFor=RuntimeException.class)         //控制事务遇到哪些异常才会回滚。默认是RuntimeException
@Transactional(rollbackForClassName=RuntimeException)      //同上
@Transactional(noRollbackFor=NullPointerException.class)   //控制事务遇到哪些异常不会回滚。默认遇到非RuntimeException不会回滚
@Transactional(noRollbackForClassName=NullPointerException)//同上
```













