---
title: MySql随机函数
date: 2017-09-11 16:51:12
categories: [Mysql]
tags: [Mysql]
description: rand()/round()/floor()/ceiling()
copyright: true
comments: true
---
<!-- more -->
> rand（）函数只能生成0到1之间的随机小数，如果想要生成0到10,0到100就rand（）*相应的值。
> 如果想得到整数就要用到round（x），floor（x）和ceiling（x）。
> round（x）是四舍五入 ； floor（x）是去小于等于x的整数； ceiling（x）是取大于等于x的整数；
> 得到指定范围的随机数 round（rand（）*（max-min）+min）即可


## ROUND
*  ROUND(X) -- 表示将值 X 四舍五入为整数，无小数位
* ROUND(X,D) -- 表示将值 X 四舍五入为小数点后 D 位的数值，D为小数点后小数位数。若要保留 X 值小数点左边的 D 位，可将 D 设为负值。
```
SELECT ROUND('123.456')
-----------------------------
123
SELECT ROUND('123.654')
-----------------------------
124
SELECT ROUND('123.456',2)
-----------------------------
123.46
SELECT ROUND('123.654',2)
-----------------------------
123.65
```
## FLOOR
* FLOOR(X)表示向下取整，只返回值X的整数部分，小数部分舍弃。
```
SELECT FLOOR('123.456')
-----------------------------
123
SELECT FLOOR('123.654')
-----------------------------
123
```
## CEILING
* CEILING(X) 表示向上取整，只返回值X的整数部分，小数部分舍弃。
```
SELECT CEILING('123.456')
-----------------------------
124

SELECT CEILING('123.654')
-----------------------------
124
```

