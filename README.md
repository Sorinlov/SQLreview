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

/*
+
only used as operand
1. 100 + 2 = 102
2. '100' + 2 = 102 -- try to change str to number
3. 'suyu' + 6 = 6 -- if fail then as 0
4. null + 6 = 0 -- as long as there is null, result is 0
*/
select last_name + first_name as name from table; -- 0
select concat(lasname + firstname) as name from table;

```
## condition select
``` SQL
select * from table
where
-- operand 
< = > != <>
-- logic
and or not
-- vague
like
/*
like '%a%' : include a 
通配符 
% 任意多个字符
_ 任意一个字符

转义 
\
任意一个字符 c escape 'c'

-- whose second str is _
like '_\_%'; 
like '_$_%' escape '$';
*/
in
where job_id in ('a', 'b', 'c') --type of elements in list should be same

between and -- [] 包含临界值； 前后顺序有意义
is null
<=> -- 安全等于 既可以判断null也可以判断普通数值

```

## sorting select 
```SQL
select * from table where 
order by filed1 desc, filed2 asc;

-- 按别名排序
select salary*12 as anualSalary from table
order by anualSalary desc;
```


## excution order
1. from 定位到表
2. where 筛选条件
3. select
4. order by 

## common functions
``` SQL
--1. 单行函数
--字符函数
concat(c1, ' ', c2)
length --汉字占三个字节
upper, lower

substr(str, pos) --索引从1开始
substr(str, pos, len)

instr(str1, str2) -- 返回str2在str1中第一次出现的位置 无则返回0

trim() -- remove space at end and begin
trim('s' from str) -- remove s at end and begin

lpad(c1, length, 's') -- 左填充
rlad() -- 右填充

--数学函数
round(double, length)
ceil() -- min int >=
floor()
truncate(double, length)
mod() -- a-a/b*b

--日期函数
now()
curdate()
curtime()
year()
month()
monthname() -- 

str_to_date
| %Y | 4位年份    |
| %y | 2位年份    |
| %m | 月份 01 02 |
| %c | 月份 1 2   |
| %d | 日         |
| %H | 24小时     |
| %h | 12小时     |
| %i | 分钟       |
| %s | 秒         |

--2. 聚合函数

```
| %Y | 4位年份    |
| %y | 2位年份    |
| %m | 月份 01 02 |
| %c | 月份 1 2   |
| %d | 日         |
| %H | 24小时     |
| %h | 12小时     |
| %i | 分钟       |
| %s | 秒         |


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

