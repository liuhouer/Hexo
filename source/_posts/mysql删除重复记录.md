title: "Mysql删除重复记录"
date: 2015-12-18 23:37:44
tags: [sql]
categories: sql
---

Mysql删除重复记录



	DELETE FROM posts WHERE id IN (
	    SELECT * FROM (
	        SELECT id FROM posts GROUP BY id HAVING ( COUNT(id) > 1 )
	    ) AS p
	)
