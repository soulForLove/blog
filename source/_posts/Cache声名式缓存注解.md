---
title: Cache声名式缓存注解
date: 2018-03-11 18:06:38
updated: 2018-03-11 18:06:38
categories: [cache]
tags: [cache]
description: Cache 声名式缓存注解
copyright: true
comments: true
---
<!--more-->

## @Cacheable
在方法执行前Spring先查看缓存中是否有数据，如果有，则直接返回缓存数据；若没有，调用方法并将方法返回值放入缓存
* value：缓存的名称，在 spring 配置文件中定义，必须指定至少一个	例如：
@Cacheable(value=”mycache”) 或者 
@Cacheable(value={”cache1”,”cache2”}
* key：缓存的 key，可以为空，如果指定要按照 SpEL 表达式编写，如果不指定，则缺省按照方法的所有参数进行组合	例如：
@Cacheable(value=”testcache”,key=”#userName”)
* condition：缓存的条件，可以为空，使用 SpEL 编写，返回 true 或者 false，只有为 true 才进行缓存	例如：
@Cacheable(value=”testcache”,condition=”#userName.length()>2”)

## @CachePut
无论怎么样，都会把方法的返回值放进缓存中，属性与@Cacheable一致

## @CacheEvict
将一条或者多条数据从缓存中删除
* value：缓存的名称，在 spring 配置文件中定义，必须指定至少一个	例如：
@CachEvict(value=”mycache”) 或者 
@CachEvict(value={”cache1”,”cache2”}
* key：缓存的 key，可以为空，如果指定要按照 SpEL 表达式编写，如果不指定，则缺省按照方法的所有参数进行组合	例如：
@CachEvict(value=”testcache”,key=”#userName”)
* condition：缓存的条件，可以为空，使用 SpEL 编写，返回 true 或者 false，只有为 true 才清空缓存	例如：
@CachEvict(value=”testcache”, condition=”#userName.length()>2”)
* allEntrie：是否清空所有缓存内容，缺省为 false，如果指定为 true，则方法调用后将立即清空所有缓存	例如：
@CachEvict(value=”testcache”,allEntries=true)
* beforeInvocation：是否在方法执行前就清空，缺省为 false，如果指定为 true，则在方法还没有执行的时候就清空缓存，缺省情况下，如果方法执行抛出异常，则不会清空缓存	例如：
@CachEvict(value=”testcache”，beforeInvocation=true)

## @Caching
可以通过@Caching注解将多个注解策略组合到一个方法上
@Caching注解可以让我们在一个方法或者类上同时指定多个Spring Cache相关的注解。其拥有三个属性：cacheable、put和evict，分别用于指定@Cacheable、@CachePut和@CacheEvict。
```
@Caching(cacheable = @Cacheable("users"), evict = { @CacheEvict("cache2"),
         @CacheEvict(value = "cache3", allEntries = true) })
   public User find(Integer id) {
      return null;
   }
```