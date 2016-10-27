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
