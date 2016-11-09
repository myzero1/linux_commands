覆盖索引完成分页

```mysql
SELECT
	*
FROM
	place_touch_record
WHERE
	id IN (
		SELECT
			id
		FROM
			(
				SELECT
					r.id
				FROM
					place_touch_record r
				WHERE
					place_number IN (
						SELECT
							place_number
						FROM
							place
						WHERE
							1 = 1
					)
				AND 1 = 1
				AND place_number LIKE "53011111%"
				AND record_time >= 1477411200
				AND record_time <= 1477497599
				AND record_type = 2
				ORDER BY
					r.id DESC
				LIMIT 0,
				20
			) AS tr
	)
```

查重复和删除重复

```mysql


SELECT
	*
FROM
	customer_record
WHERE
	(
		id_number,
		online_time,
		offline_time
	) IN (
		SELECT
			r1.id_number,
			r1.online_time,
			r1.offline_time
		FROM
			(
				SELECT
					*
				FROM
					`customer_record`
			) AS r1
		GROUP BY
			r1.id_number,
			r1.online_time,
			r1.offline_time
		HAVING
			count(*) > 1
	)
AND id not IN (
	SELECT
		min(r1.id)
	FROM
		(
			SELECT
				*
			FROM
				`customer_record`
		) AS r1
	GROUP BY
		r1.id_number,
		r1.online_time,
		r1.offline_time
	HAVING
		count(*) > 1
)
ORDER BY
	id_number






SELECT
	group_concat(id)
FROM
	customer_record
WHERE
	(
		id_number,
		online_time,
		offline_time
	) IN (
		SELECT
			r1.id_number,
			r1.online_time,
			r1.offline_time
		FROM
			(
				SELECT
					*
				FROM
					`customer_record`
			) AS r1
		GROUP BY
			r1.id_number,
			r1.online_time,
			r1.offline_time
		HAVING
			count(*) > 1
	)
AND id NOT IN (
	SELECT
		min(r1.id)
	FROM
		(
			SELECT
				*
			FROM
				`customer_record`
		) AS r1
	GROUP BY
		r1.id_number,
		r1.online_time,
		r1.offline_time
	HAVING
		count(*) > 1
)
ORDER BY
	id_number








DELETE
FROM
	customer_record
WHERE
	(
		id_number,
		online_time,
		offline_time
	) IN (
		SELECT
			r1.id_number,
			r1.online_time,
			r1.offline_time
		FROM
			(
				SELECT
					*
				FROM
					`customer_record`
			) AS r1
		GROUP BY
			r1.id_number,
			r1.online_time,
			r1.offline_time
		HAVING
			count(*) > 1
	)
AND id not IN (
	SELECT
		min(r1.id)
	FROM
		(
			SELECT
				*
			FROM
				`customer_record`
		) AS r1
	GROUP BY
		r1.id_number,
		r1.online_time,
		r1.offline_time
	HAVING
		count(*) > 1
)


```

存在则更新不存在则插入

```mysq

INSERT INTO customer_record (
	online_time,
	offline_time,
	net_ending_name,
	net_ending_ip,
	place_number,
	id_number,
	record_date
)
VALUES
	(
		'1476683352',
		'1476699352',
		'PC-11',
		'192.168.3.4',
		'33112112',
		'123456789012345678',
		'2016-10-28'
	),
	(
		'1476683359',
		'1476683361',
		'PC-11',
		'192.168.3.4',
		'33112112',
		'123456789012345679',
		'2016-10-29'
	) ON DUPLICATE KEY UPDATE customer_record.offline_time =
VALUES
	(
		customer_record.offline_time
	),
customer_record.record_date =
VALUES
	(
		customer_record.record_date
	)

```

MySQL数据表修复

```mysql

常用的Mysql数据库修复方法有下面3种:

1. mysql原生SQL命令: repair

　　即执行REPAIR TABLE SQL语句

　　语法：REPAIR TABLE tablename[,tablename1...] [options]

　　示例: mysql> use database xxx;

　　　　mysql> repair table *;

 

2.使用MySQL自带的客户端工具: myisamchk (无需停止MySql服务)

具体信息可见: http://dev.mysql.com/doc/refman/5.1/zh/client-side-scripts.html#mysqlcheck

有3种方式来调用mysqlcheck：

shell> mysqlcheck[options] db_name [tables]
shell> mysqlcheck[options] ---database DB1 [DB2 DB3...]
shell> mysqlcheck[options] --all--database
示例:

自动检查并修复数据库xxxdatabase的所有表:

 

shell> mysqlcheck --auto-repiar xxxdatabase -uroot -p

 

3.使用MySQL自带的客户端工具: myisamchk (需要停止MySql服务)
　　较少使用, 如果想了解详细, 请见Mysql官方文档.
  
```

