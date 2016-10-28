覆盖所有完成分页

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
ORDER BY
	id_number


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
