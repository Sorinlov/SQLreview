# SQLreview

``` SQL
show databases 

use database

show tables
show tables from database

desc table 
-- show data scehma infomation
```

## create table 
```SQL
create table newtable as 
(
select * from 
)

create table newtable {
   c1 type,
   c2 type,
   ...
}
```
## basic select 
``` SQL
select 100;
select 'suyu' as "handsome guy";
select 100%99 as result;
select version(); --版本
-- select constant, function and expression 

-- distinct

-- + 
select last_name 
```

## join

* left join
* right join
* inner join
* full outer join

* a 中有 b 中没有                                               
```SQL
select *
from a join b on a.id = b.id
where b.id is null
```

* a b 的异或集
```SQL
select *
from a join b on a.id = b.id
where a.id is null or b.id is null
```

*  笛卡尔积join
```SQL
select *
from a cross join b 
```

## 行列转换

* 行转列
```SQL
select name '姓名',
max(case when subject = '语文' then result else 0) end as '语文',
max(case when subject = '数学' then result else 0) end as '数学',
cast(avg(result*1.0) as decimal(18,2)) '平均分',
sum(result) '总分'
from tbl 
group by name
```

```SQL
--pivot
SELECT 
   '班级总人数:'AS[总人数],
   [初一 1班], [初一 2班],
   [初二 1班],
   [初三 1班], [初三 2班]
FROM (
   SELECT 
      [所属班级]AS[班级], 
      [学生编号]
   FROM #Student 
) AS[SourceTable]
PIVOT (
   COUNT([学生编号])  -- 聚合函数
   FOR[班级]IN ( -- 需要转化的列
      [初一 1班], [初一 2班],  --新的列名
      [初二 1班],
      [初三 1班], [初三 2班]
   )
) AS[PivotTable]
```

* 列转行
```SQL
select * from 
( 
  select 姓名 as Name , Subject = '语文' , Result = 语文 from tb1 
  union all 
  select 姓名 as Name , Subject = '数学' , Result = 数学 from tb1 
  union all 
  select 姓名 as Name , Subject = '物理' , Result = 物理 from tb1 
) t 
order by name
```

```SQL
-- unpivot
SELECT 
   [班级], [总人数]
FROM (
   SELECT 
      [初一 1班], [初一 2班],
      [初二 1班],
      [初三 1班], [初三 2班]
   FROM 
      #PivotTable
) AS[s]
UNPIVOT (
   [总人数]
   FOR[班级]IN (
      [初一 1班], [初一 2班],
      [初二 1班],
      [初三 1班], [初三 2班]
   )
) AS[un_p]
```

## rownumber 以及 取第一行
```SQL
row_number() over(partition by ... order by ... ) as rownum
where rownum = 1
```  

## 上下偏移

* lag()
```SQL
select lag(name, 1, default) over(partition by ... order by ...) from 
```
* lead()
```SQL
select lead(name, 1, default) over(partition by ... order by ...) from

