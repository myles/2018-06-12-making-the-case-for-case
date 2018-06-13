autoscale: true
theme: Zurich, 5

# Making the case for `CASE`

## or: Why I Love Conditional Expressions in Postgres, SQLite, and MySQL

### A lighting talk by [Myles Braithwaite](https://mylesb.ca/)

---

# I like to think of Conditional Expressions like `if`, `elif`, and `else` logic statements in SQL.

---

| pk  | name      | level   |
| --- | ---       |  ---    |
| 101 | Caelestis | GOLD    |
| 102 | Zoticus   | SILVER  |
| 103 | Willihard | NULL    |

---

```sql
SELECT
  `pk`,
  `name`,
  `level`,
  CASE `level`
    WHEN 'GOLD' THEN 0.20
    WHEN 'SILVER' THEN 0.10
    ELSE 0
  END AS `discount`
FROM
  `customers`;
```

---

| pk  | name      | level   | discount |
| --- | ---       |  ---    | ---      |
| 101 | Caelestis | GOLD    | 0.20     |
| 102 | Zoticus   | SILVER  | 0.10     |
| 103 | Willihard | NULL    | 0        |

---

# Let's do something _weird_.

---

| pk  | name   | price |
| --- | ---    | ---   |
| 101 | Apple  | 1     |
| 102 | Banana | 1.5   |
| 103 | Coffee | 0.5   |

---

```sql
SELECT
  `customers`.`name`,
  `customers`.`level`
  1 - (
    CASE level
      WHEN 'GOLD' THEN 0.20
      WHEN 'SILVER' THEN 0.10
      ELSE 0
    END * (
      SELECT `products`.`price`
  		FROM `products`
		  WHERE `products`.`name` = 'Apple'
  	)
  ) AS `total`
FROM
	`customers`;
```

---

| name      | level  | total |
| ---       | ---    | ---   |
| Caelestis | GOLD   | 0.8   |
| Zoticus   | SILVER | 0.9   |
| Willihard | NULL   | 1     |

---

| pk  | name      | level   |
| --- | ---       |  ---    |
| 101 | Caelestis | GOLD    |
| 102 | Zoticus   | SILVER  |
| 103 | Willihard | NULL    |

---

```sql
SELECT
	`name`
FROM
  `customers`
ORDER BY
  `name` ASC;
```

---

| name      |
| ---       |
| Caelestis |
| Zoticus   |
| Willihard |

---

```sql
SELECT
  `name`,
  CASE
    WHEN `name` LIKE 'W%' THEN 1
    ELSE 0
  END AS `vip`
FROM
  `customers`
ORDER BY
  `vip` DESC,
  `name` ASC
```

---

| name      |
| ---       |
| Willihard |
| Caelestis |
| Zoticus   |
