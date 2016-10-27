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
