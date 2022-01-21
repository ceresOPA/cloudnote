# SQL

> 学习资源：[自学SQL网](http://xuesql.cn/)

?> 各语句写的顺序：

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

?> 各语句执行顺序：

1. from和join：确定一个数据源表(临时表)
2. where：对数据源进行筛选
3. group by：分组
4. having：分组后的数据进行筛选
5. select：确定输出
6. distinct：排重
7. order by：排序（已经过select的计算后，可使用**col_name**的as的别名，之前都不能使用as别名）**table是可以使用别名的**:imp:
8. limit/offset：截取部分数据

!> 这里补充一下关于执行顺序的问题，因为在牛客刷SQL中发现group by中竟然可以使用列的别名，然后看到讨论区给出的理由是：**因为MySQL对查询做了增强没有严格遵循SQL的执行顺序，where后面不能用select中的别名，但是group by ，order by都是可以的，Oracle数据库严格遵循了SQL执行顺序在Oracle里group by是不能引用select里的别名的。**

## 基础篇

### SQL条件查询

#### 1. 使用WHERE语句筛选数字类型的属性

> 把不太熟悉的sql条件查询的语句记一下，之前除了=、!=这种常用的，很少用到下面的四种

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220118220927157.png" alt="image-20220118220927157" style="zoom: 50%;" />

?> 这里注意下，BETWEEN...AND...，是包含两侧的值的，如

```sql
between 20 and 23 --是包括20和23的
```



#### 2. 字符串相关筛选

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220118223539058.png" alt="image-20220118223539058" style="zoom:50%;" />

?> 关于字符匹配中，可能存在的大小写问题，可以通过将字符串先全部转换为大写或小写，然后再去匹配：

```sql
select * from movies where upper(title) like "%STRING%";
select * from movies where lower(title) like "%string%";
```



### SQL筛选与排序

#### 1. distinct 与 group by

!> 目前关于具体的区别还没有很清楚，后续更新

| 语句                                  | 功能                                           |
| ------------------------------------- | ---------------------------------------------- |
| select distinct col_name from table   | 筛选某一字段，去除重复内容                     |
| select * from table group by col_name | 分组，也可去除某列重复内容，同时还可对组内求和 |

- group by 的多列用法

?> 当需要对已经按某一列分组的数据再按另外一列进行进一步的分组的话，可以通过下面的语句实现

```sql
SELECT building_name,role FROM buildings left join employees
on buildings.building_name = employees.building
group by building_name,role;
-- 执行上面的语句后，会先按建筑分组，然后再对同一建筑内的不同角色进行分组
```

#### 2. 排序(order by)

?> order by col_name(asc/desc), 根据某一列进行排序<br />数值类型的列：直接按照数值大小排序（asc：9 8 7... ；desc：1 2 3...）<br />字符类型的列：按照字母表的顺序进行排序

```sql
select * from movies order by id asc; --默认是asc升序
```

#### 3. 筛选(limit  ; offset)

?> 可以用于某些情况下的分页查询，避免一次性查询大量数据

| 语句   | 功能                                                         |
| ------ | ------------------------------------------------------------ |
| limit  | 取多少条记录                                                 |
| offset | 从哪一条开始取（默认从0开始，因此offset 2，则从第三条开始取） |

```sql
select * from movies 
where title like "amazing"
order by id desc
limit 5 offset 5;
-- 表示取id最高的第6到第10个
```



### 多表的连接

#### 1. 内连接(inner join)

- inner join，也可直接简写为join，相当于两表的交集（如A和B，则为A∩B）
- 图示（A∩B）：![AB](https://gitee.com/y255413580/img/raw/master/noteimg/AB.png)

- 作用：通过内连接得到的新表只要原来的表不存在null，则不会产生新的null。但显然，因为是交集，所以没有匹配的记录会直接被丢弃。
- 语句：

```sql
select * from table_A inner join table_B
on table_A.id = table_B.A_id
```

#### 2. 外连接(outer join)

- outer join，分为三种——left outer join(左外连接)、right outer join(右外连接)和full outer join(全连接)，都可以将outer简写掉，直接为left join,right join和full join。
- 图示：
  - left join（A-B）  ：![A-B](https://gitee.com/y255413580/img/raw/master/noteimg/A-B.png)
  - right join（B-A）：![B-A](https://gitee.com/y255413580/img/raw/master/noteimg/B-A.png)
  - full join（A∪B）：![A∪B](https://gitee.com/y255413580/img/raw/master/noteimg/A%E2%88%AAB.png)
- 作用：这三种外连接，都有可能导致null的出现，对于左连接，会保留A表的所有记录，则会导致没有与B表对应的该条记录的B的字段值为null，另外两种类似。
- 语句：

```sql
select * from table_A left/right/full join table_B
on table_A.id = table_B.A_id
```



### 关键字NULL的使用

数据库中null的出现经常是不可避免的，我们可以通过is null 或 is not null的方式对存在null值字段的表进行筛选。

```sql
-- 在查询条件中处理 NULL
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

?> 有的时候也可以**在建表的时候给某些不愿出现null的字段设置默认值**，如数值型的某些字段可以设置默认值为0，字符串型的可以设置为""，当然这并不唯一，要结合具体情形具体分析。



### sql查询中的表达式

?> 在查询的列和where语句中的列都是可以使用表达式的，且表达式不限于普通的+-*/，一些数据库也提供了一些函数以供调用，如count()，str()等函数

```sql
-- 包含表达式的例子
SELECT  particle_speed / 2.0 AS half_particle_speed -- (对结果做了一个除2）
FROM physics_data
WHERE ABS(particle_position) * 10.0 >500;
            --（条件要求这个属性绝对值乘以10大于500）
```



### 分组后的条件查询(HAVING)

因为where语句的执行在group by之前，即是对表进行筛选后再进行的分组。因此，若想在分组后再对各组进行筛选则是无法通过where语句来是实现的，此时就需要使用到HAVING语句。

语句：

```sql
select role,count(*) from officer
group by role
having role like "Engineer"
-- 用于查询角色为Engineer的人数，即先按角色分组，再筛选出角色为Engineer的记录
```



## 进阶篇

### Union操作

?> 通过union操作，可以实现在有相同字段的多条记录的合并操作，可以认为是增加行

```sql
select username,gender,age from users where gender like "male" and age>22 union
select username,gender,age from users where gender like "female" and age>20;
-- 查找男性大于22岁的或者女性大于20岁的用户
-- 当然也可以使用OR来实现该查询
```

#### 关于UNION ALL的使用（避免去重）

union的默认操作其实就类似or，当满足两个条件的时候，则会自动进行去重只保留一条记录，但某些情况下可能并不希望他去重，那么就需要使用到union all，来避免自动去重。【习题可以参考：[牛客SQL25](https://www.nowcoder.com/practice/979b1a5a16d44afaba5191b22152f64a?tpId=199&tags=&title=&difficulty=0&judgeStatus=0&rp=0)】

```sql
select device_id,gender,age,gpa from user_profile where university like "山东大学" union all
select device_id,gender,age,gpa from user_profile where gender like "male";
```

### 多表的连接

多表的连接，其实与两个表的连接是类似的，只需在后面再join就可以了

```sql
-- 这下面是两个左连接
select university,difficult_level,round(count(*)/count(distinct q.device_id),4) as avg_answer_cnt
from question_practice_detail q
left join user_profile u on q.device_id = u.device_id
left join question_detail qu on q.question_id = qu.question_id
where university like "山东大学"
group by university,difficult_level
```



### 常用函数

#### 数值函数

| 函数                               | 作用                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| ROUND(col_name,四舍五入保留位数)   | select round(gpa,1) from user_profile<br/>-- gpa保留1位小数  |
| TRUNK(col_name,保留小数位数)       | select trunk(gpa,1) from user_profile<br/>-- trunk是直接截断，如3.1415，保留3位，则直接去掉5 |
| CEILING(col_nam) / FLOOR(col_name) | 向上/向下取整函数                                            |
| ABS(col_name)                      | 绝对值函数                                                   |
| RAND()                             | 随机数0-1                                                    |
| MAX(col_name)                      | select max(gpa) from user_profile<br/>-- 查询gpa最大的记录   |
| AVG(col_name)                      | select avg(gpa) from user_profile<br/>-- 求出所有用户的平均gpa |
| SUM(col_name)                      | select sum(gpa) from user_profile<br/>-- 求出所有用户的gpa总和 |

#### 字符函数

| 函数                                                 | 作用                                       |
| ---------------------------------------------------- | ------------------------------------------ |
| LENGTH()                                             | 返回字符串的长度                           |
| UPPER() / LOWER()                                    | 全部大写或小写                             |
| LTRIM() / RTRIM() / TRIM()                           | 去除左空格/右空格/全部空格                 |
| LEFT(“string”,左侧多少个字符)                        | left("string",3)<br />--- 输出 str         |
| RIGHT("string",右侧多少个字符)                       | right("string",3)<br />--- 输出 ing        |
| SUBSTRING("string",起始字符位置，结束字符位置[可选]) | substring("string",2,5)<br />--- 输出trin  |
| LOCATE(子串,字符串)                                  | 返回子串第一次出现的位置                   |
| REPLACE(字符串，内容，替换内容)                      |                                            |
| CONCAT(连接多个字符串)                               | concat(str1,str2,str3)                     |
| substring_index(str,delim,count)                     | str:要处理的字符串 delim:分隔符 count:计数 |



#### 日期函数

| 函数                                        | 作用                          |
| ------------------------------------------- | ----------------------------- |
| NOW()                                       | 返回类似：2022-01-21 16:40:24 |
| CURDATE()                                   | 返回类似：2022-01-21          |
| CURTIME()                                   | 返回类似：16:40:24            |
| YEAR(NOW())                                 | 返回类似：2022                |
| MONTH(NOW())                                | 返回类似：1                   |
| 同类的还有DAY()、HOUR()、MINUTE()、SECOND() | 天、时、分、秒                |



#### 特殊函数

| 函数                                   | 作用                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| IFNULL(col_name,"替换内容")            | select IFNULL(age,"未知") from users <br/>-- 用于表示如果age为空，则替换为未知 |
| COALESCE(col_name,col_name,"替换内容") | 从左往右，返回第一个不为空的内容                             |

#### IF与CASE的使用

- IF函数

```sql
select IF(age>=25,"25岁及以上","25岁以下") as age_cut from user_profile
```

IF(条件，为真返回的值，为假返回的值)    和Excel有点像。

- CASE函数

```sql
select device_id,gender,
CASE
    WHEN age <20 THEN "20以下"
    WHEN age between 20 and 24 THEN "20-24岁"
    WHEN age >=25 THEN "25岁及以上"
    ELSE "其他"
END AS age_cut
from user_profile;
```

注意下格式：

```sql
CASE
	WHEN ... THEN ...
	...
	ELSE ...
END AS ...
```



### count的计数问题

count()函数，按照之前设想的是如果有count(result="true")，则会统计的应该是result="true"的字段，但是会发现并没有正常统计，因为count()函数不会统计的值是null，所以这里要写成count(case when result="true" then 1 else null end)才会达到想要的效果，或者用if也可以count(IF(result="true",1,null))



### 多排序的设置

和group by类似，order by后面也可以接不止一个字段，如order by filed1,filed2，用于表示先按照filed1字段升序，再按照filed2字段升序，这里默认都是asc升序，关于使用asc升序还是desc降序都是可以单独设置的

```sql
select device_id,gpa,age from user_profile
order by gpa desc,age desc;
-- 这里分别将两个设置为降序，如果不设置，则默认是升序asc
```





## 问题篇

**一、按角色分组算出每个角色按有办公室和没办公室的统计人数(列出角色，数量，有无办公室,注意一个角色如果部分有办公室，部分没有需分开统计）**

【题目链接：http://xuesql.cn/lesson/select_queries_with_aggregates_pt_2】

![image-20220120142522481](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220120142522481.png)

- 思路：这道题一开始写的时候想的是按角色分组后然后通过筛选出是否有办公室，但是再回过头看题目会发现题目要求是说对角色分组后，分开统计有无办公室的人数，所以貌似用where或者having得不到结果（网上有个使用到where的语句，不过没怎么看懂，下面贴出解法）

- 预期结果：

  | role     | count | bn   |
  | -------- | ----- | ---- |
  | Engineer | 5     | 1    |
  | Engineer | 1     | 0    |
  | Artist   | 5     | 1    |
  | Artist   | 5     | 0    |
  | Manager  | 6     | 1    |

- 自己的解法：

  ?> 感觉关键其实就只是在于group by的用法，group by是可以实现多次分组的，先按role分组，然后再从各role里按building is null分组。

  ```sql
  select role,count(*),(building is not null) as bn 
  from employees
  group by role,(building is not null)
  ```

- 网上解法（链接：https://blog.csdn.net/qq_36848833/article/details/104327930）：

  ```sql
  SELEct
  Role
  ,case when building is null then “1”
  else “0” end as 有无办公室
  ,count(Name)
  FROM employees where 1 group by role,有无办公室
  ```





**二、SQL21** **浙江大学用户题目回答情况**

【题目链接：[牛客基础SQL21](https://www.nowcoder.com/practice/55f3d94c3f4d47b69833b335867c06c1?tpId=199&tags=&title=&difficulty=0&judgeStatus=0&rp=0)】

- 思路：题目本身不难，先表连接，然后再筛洗就可以得到结果。但是，从讨论中看到另外一种解法，是在where中使用到了device_id相等的条件。不过主要还是不怎么理解f**rom两个表后得到的是什么样的一个临时表**。
- **补充：**<font color="red" >select * from table_A,table_B; 其实就是对table_A和table_B进行内连接</font>
- 自己的解法：

```sql
select question_practice_detail.device_id as device_id,question_id,result from question_practice_detail left join 
user_profile on question_practice_detail.device_id = user_profile.device_id
where university like "浙江大学"
order by question_id asc;
```

- 讨论区解法：

```sql
SELECT u.device_id,q.question_id,q.result
from question_practice_detail q,user_profile u
where university = '浙江大学'and q.device_id=u.device_id
```





**三、mysql使用group by查询报错SELECT list is not in GROUP BY clause and contains nonaggregated column...解决方案**

【题目链接：[牛客SQL24](https://www.nowcoder.com/practice/f4714f7529404679b7f8909c96299ac4?tpId=199&tags=&title=&difficulty=0&judgeStatus=0&rp=0)】

- 思路：这里题目要求是查询**参加了答题**的**山东大学**的用户在**不同难度**下的**平均答题题目数**，所以，这里一开始是容易想到的通过将答题表和用户表与答题详情表进行左连接，然后通过where筛选出只有山东大学的用户，再通过对学校和难度进行group by。

  不过，这里在进行group by的时候，我就在想既然既然经过了where筛选后，那不就只有山东大学的用户吗？那不就只需要对难度进行分组，不需要再对学校进行分组了，就只有group by difficult_level，结果就报出了上面的错误，根据查询了解到说是mysql5.7.5后使用了only_full_group_by作为默认的sql_mode，不过这个语句确实是存在问题的，**因为若表中有不同的学校相同的题目难度的记录，那group by difficult_level后，select university是没有知道是哪个大学的。**

- 正解：

```sql
select university,difficult_level,round(count(*)/count(distinct q.device_id),4) as avg_answer_cnt
from question_practice_detail q
left join user_profile u on q.device_id = u.device_id
left join question_detail qu on q.question_id = qu.question_id
where university like "山东大学"
group by university,difficult_level;
```

- 另解（<font color="orange">突然想到，用having也可以解决</font>）：

```sql
select university,difficult_level,round(count(*)/count(distinct q.device_id),4) as avg_answer_cnt
from question_practice_detail q
left join user_profile u on q.device_id = u.device_id
left join question_detail qu on q.question_id = qu.question_id
group by university,difficult_level
having university like "山东大学"
```

- 关于only_full_group_by参考资料：https://blog.csdn.net/study_in/article/details/92625397



