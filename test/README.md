https://www.espncricinfo.com/series/australia-in-pakistan-2021-22-1288300/pakistan-vs-australia-2nd-odi-1288314/full-scorecard

```sql
SELECT DISTINCT(batsman),
(SELECT SUM(runs) FROM match WHERE batsman = m.batsman AND type NOT IN ('leg bye', 'wide')) "R"
FROM match m
WHERE innings = 1;
```
```sh
   batsman   |  R  |  B  | 4s | 6s |   SR
-------------+-----+-----+----+----+--------
 Head        |  89 |  70 |  6 |  5 | 127.14
 Finch       |   0 |   1 |  0 |  0 |   0.00
 McDermott   | 104 | 108 | 10 |  4 |  96.30
 Labuschagne |  59 |  49 |  5 |  0 | 120.41
 Stoinis     |  49 |  33 |  5 |  1 | 148.48
 Carey       |   5 |  10 |  0 |  0 |  50.00
 Green       |   5 |  11 |  0 |  0 |  45.45
 Abbott      |  28 |  16 |  4 |  0 | 175.00
 Ellis       |   1 |   1 |  0 |  0 | 100.00
 Zampa       |   0 |   1 |  0 |  0 |   0.00
(10 rows)
```

```sh
       bowler        | O  | M | R  | W | ECON | 0  | 4s | 6s | WD | NB
---------------------+----+---+----+---+------+----+----+----+----+----
 Shaheen Shah Afridi | 10 | 0 | 63 | 4 | 6.30 | 30 |  6 |  2 |  0 |  0
 Haris Rauf          | 10 | 1 | 57 | 0 | 5.70 | 29 |  6 |  1 |  0 |  0
 Mohammad Wasim      | 10 | 0 | 56 | 2 | 5.60 | 31 |  6 |  1 |  0 |  0
 Zahid               | 10 | 0 | 71 | 1 | 7.10 | 20 |  7 |  1 |  1 |  0
 Iftikhar            |  4 | 0 | 38 | 0 | 9.50 |  5 |  1 |  3 |  0 |  0
 Khushdil            |  6 | 0 | 57 | 1 | 9.50 |  7 |  4 |  2 |  1 |  0
(6 rows)
```
