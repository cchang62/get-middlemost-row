# get-middlemost-row

```sql
--Solution 1
select *
from [DB].[Schema].[Table1]
order by [Column1]
offset (select count(*) / 2 - (case when count(*) % 2 = 0 then 1 else 0 end) from  [DB].[Schema].[Table1]) rows
fetch next (select 2 - (count(*) % 2) from  [DB].[Schema].[Table1]) rows only 


--Solution 2
select h.*
from (select h.*, row_number() over (order by [Column1]) as [Alias1],
             count(*) over () as cnt
      from [DB].[Schema].[Table1] h
     ) h
where 2 * [Alias1] in (cnt, cnt - 1, cnt + 1); 


------------------------with capital
--Solution 1
SELECT *
FROM [DB].[Schema].[Table1]
ORDER BY [Column1]
OFFSET (SELECT COUNT(*) / 2 - (CASE WHEN COUNT(*) % 2 = 0 THEN 1 ELSE 0 END) FROM [DB].[Schema].[Table1]) ROWS
FETCH NEXT (SELECT 2 - (COUNT(*) % 2) FROM  [DB].[Schema].[Table1]) ROWS ONLY 


--Solution 2
SELECT h.*
FROM (SELECT h.*, ROW_NUMBER() over (ORDER BY [Column1]) AS [Alias1],
             COUNT(*) OVER () AS cnt
      FROM [DB].[Schema].[Table1] h
     ) h
WHERE 2 * [Alias1] IN (cnt, cnt - 1, cnt + 1); 


```
