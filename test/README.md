https://www.espncricinfo.com/series/australia-in-pakistan-2021-22-1288300/pakistan-vs-australia-2nd-odi-1288314/full-scorecard

```sql
SELECT DISTINCT(batsman),
(SELECT SUM(runs) FROM match WHERE batsman = m.batsman AND type NOT IN ('leg bye', 'wide')) "R"
FROM match m
WHERE innings = 1;
```
