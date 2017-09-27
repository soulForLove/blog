---
title: Mysql小知识
date: 2017-09-13 09:50:01
categories: [Mysql]
tags: [Mysql,小知识]
description: Mysql 相关知识点
copyright: true
comments: true
---
<!-- more -->
1. Count(null) = 0
2. Sum(null) = null
3. null + 1 = null
4. 根据得分进行排名，分数为0 不进行排名
```
SELECT
	t.competition_id,
	t.player_id,
	(
		SELECT
			CASE WHEN t.score <= 0 THEN NULL ELSE COUNT(DISTINCT score) END # 当score小于等于0时，设定排名为null，否则根据where条件（t1.score > t.score）count不同的score值来获取排名
		FROM
			v_player_total_data t1
		WHERE
			t1.competition_id = t.competition_id
		AND t1.score > t.score
	) + 1 score
FROM
	v_player_total_data t ORDER BY competition_id

```
