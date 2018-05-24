---
title: Redis常见数据类型操作命令
date: 2018-05-17 10:46:25
updated: 2018-05-17 10:46:25
categories: redis
tags: redis
description: redis常用五种数据类型:string,hash,list,set,zset(sorted set).
copyright: true
comments: true
---
<!--more-->
## Key
* keys *---查询所有key
* exists key---判断key是否存在
* move key db---移除当前库的key值
* expire key  秒钟---设定过期时间
* ttl key ---查看多少秒后过期，-1永不过期，-2已过期
* type key ---查看key类型
* FLUSHALL---清空所有数据库
## String
* set/get/del/append（追加）/strlen（获取key长度）
* incr（递增1）/decr（递减1）/incrby（指定递增多少）/decrby（指定递减多少）---数字才行
* getrange（区间范围内取值）/setrange（区间范围内覆盖设值）
* setex（set with expire）设值以及过期时间/setnx（set if not exist 不存在则写入）
* mset（多值设置）/mget（多值获取）/msetnx（全部不存在才进行插入）
* getset 先获取再设值
## Hash
* hset/hget/hmset/hmget/hgetall/hdel
* hlen获取键值个数
* hexist key keyname 判断keyname是否在key中 
* hkeys（获取所有key）/hvals（获取所有values）
* hincrby/hincrbyfloat（指定递增）
* hsetnx（键值不存在则进行设值）
## List
* lpush（添加左边元素）/rpush（添加右边元素）/lrange（LRANGE key start stop 获取列表片段，0 -1 返回整个列表）
* lindex （返回索引的元素值，-1表示从最右边的第一位）
* llen（获取list长度）
* lrem（LREM key count value，返回被删除的个数）
count>0，从左边开始删除前count个值为value的元素
count<0，从右边开始删除前|count|个值为value的元素
count=0，删除所有值为value的元素
* ltrim （根据传入索引截取保留对应列表片段）
* rpoplpush（一个列表右移除转移另一个列表左插入）
* lset（设值元素值）
* linsert key  before/after val01 val02 （在val01之前或者之后插入val02）
## Set
* sadd（新增set元素，去重）/smembers（获取set集合）/sismember（判断是否set里的值）
* scard（返回集合元素个数）
* srem（ 删除集合中一个或多个元素，返回成功删除的个数）
* srandmember（SRANDMEMBER key [count]）
根据count不同有不同结果，count大于元素总数时返回全部元素
count>0 ，返回集合中count不重复的元素
count<0，返回集合中count的绝对值个元素，但元素可能会重复
* spop（随机出栈）
* smove（smove k1 k2 val ->将k1的val剪切到k2上）
* sdiff【差集】（sdiff A B ---->集合A和集合B，差集表示A-B，在A里有的元素B里没有，返回差集合；多个集合(A-B)-C）
* sinter【交集】
* sunion【并集】
## ZSet
* zadd（不存在添加，存在更新）
* zscore（获取元素分数）
* zrange（元素从小到大:加上withscores 返回带元素，即元素，分数，当分数一样时，按元素排序）
* zrevrange（元素从大到小）
* zrangebyscore（指定分数范围元素）
返回从小到大的在min和max之间的元素，( 左括号表示不包含，例如：80-100--->(80 100
withscore返回带分数
limit offest count 向左偏移offest个元素，并获取前count个元素
* zrevrangebyscore（从大到小排序）
* zrem（删除元素）
* zcard（计算集合内个数）
* zcount（计算对应范围内个数）
eg：ZCOUNT salary 2000 5000          ---->计算薪水在 2000-5000 之间的人数
* zrank（获取下标值）
* zrevrank（逆序获取下标值）
* zscore（获取对应的分数）