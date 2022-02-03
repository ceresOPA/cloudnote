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

## 入门篇

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



## 基础篇

### 表的创建

#### 1. 表创建

```sql
create table if not exists user_info_vip(
    id int(11) not null primary key auto_increment comment "自增ID",
    uid int(11) not null unique comment "用户ID",
    nick_name varchar(64) comment "昵称",
    achievement int(11) default 0 comment "成就值",
    level int(11) comment "用户等级",
    job varchar(32) comment "职业方向",
    register_time datetime default CURRENT_TIMESTAMP comment "注册时间"
);
```

#### 2. 表复制

```sql
create table data_archieved as
select * from data;
```

#### 3. SQL中创建相同表的as与like的区别

【参考来源：https://blog.csdn.net/weixin_45691780/article/details/106885688】

- 语句：

  - ```sql
    create table users_record as select * from users; --- as创建相同表
    ```

  - ```sql
    create table users_record like users; --- like创建相同表
    ```

- 用途：
  - as：用来创建相同表结构，并复制原数据，可以选择字段
  - like：用来创建完整表结构**和索引**，没有数据
- 区别：
  - as：创建出来的新表缺少索引，仅保留表结构
  - like：创建的新表包含完整的表结构和索引
- 补充：MySQL中支持as和like，Oracle SQL中只支持as

### 表结构的修改

?> 下列语句中，其中有【】的表示可选

#### 1. 添加列

```sql
alter table 表名
add column 列名 列的类型
【first|after 列】 -- 添加位置，放在第一个或者放在某个字段后面
--例：
alter table users
add column avater_url varchar(50)
after password
--- 在password列后增加一列用于保存头像的url
```

#### 2. 删除列

```sql
alter table 表名
drop column 列名
--例:
alter table users
drop column comment
--- 删除comment列
```

#### 3. 修改列

##### ① 改变列名

```sql
alter table 表名
change column 旧列名 新列名 类型
--例:
alter table users
change column username nickname varchar(15)
将username修改为nickname
```

##### ②修改列的类型或约束

```sql
alter table 表名
modify column 列名 新类型
【新约束】
--例:
alter table users
modify column uid int(11)
auto_increasement
```

##### ③调整列的位置

```sql
alter table 表名
modify column 列名 类型
first|after 列名
--例:
alter table users
modify column avater_url varchar(50)
first
--- 把avatar_url字段放在首位
```



?> 其实我还挺不能理解的，为什么每次都要带上类型？即使在不需要修改列的类型的时候也要带上，就有点疑惑。

### 表的删除

#### 1. 删除某一个表

```sql
drop table if exists table_A;
```

#### 2. 同时删除多表

```sql
drop table if exists table_A,table_B,table_C;
```



### 插入数据

#### 1. 单行插入

```sql
/**
insert into table(可选字段[如果没有则按照表的默认顺序])
values ();
**/
insert into users(username,account,password)
values ('le','123456','123456')

-- 默认按表顺序，如表结构字段的顺序为 id(key，自增)，username , account , password
insert into users
values (default,'le','123456','123456') -- 这里id设置为default，就是默认值，id自增
```

#### 2. 多行插入

```sql
-- 假设表字段为  id(key，自增)，username , account , password，comment
insert into users
values (default,'le','123456','123456',null),
	   (default,'le1','1234567','1234567','comment');
```

#### 3. replace插入

```sql
/**REPLACE INTO 跟 INSERT 功能类似
不同点在于：REPLACE INTO 首先尝试插入数据到表中,
如果发现表中已经有此行数据（根据主键或者唯一索引判断）则先删除此行数据，
然后插入新的数据;否则，直接插入新数据。
要注意的是：插入数据的表必须有主键或者是唯一索引！
否则的话，REPLACE INTO 会直接插入数据，这将导致表中出现重复的数据。
**/

REPLACE INTO examination_info(exam_id,
                              tag,
                              difficulty,
                              duration,
                              release_time)
VALUES(9003,'SQL','hard',90,'2021-01-01 00:00:00');
```

摘抄自[牛客讨论区](https://www.nowcoder.com/practice/978bcee6530a430fb0be716423d84082?tpId=240&tqId=2223556&ru=/exam/oj&qru=/ta/sql-advanced/question-ranking&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D240)

#### 4.表的复制

- 将一张表中的内容插入到另一张已知表中，和普通的插入类似，可以在表后面加()选定字段，也可以按照默认的

```sql
-- 第一种
insert into data_archived
select * from data
-- 第二种
insert into data_archived(uid,account,login_date)
select uid,account,login_date from data
```

- 也可以在创建的时候复制一张表，具体见上方[表的创建中的表复制](###表的创建)



### 更新数据

#### 1.更新(update table)

```sql
update users
set username = 'le' , comment = '123456'
where user_id = 1 --- 查询中where能用的update中的也可以用，比如 user_id in (1,2,3,4,5)
```

!> 注意：① update后面没有from；② set在where前面！！！！！



### 删除数据

#### 1. 删除记录(delete)

```sql
-- 和select类似，有from
delete from exam_record   
where submit_time is null or minute(timediff(submit_time,start_time))<5 
order by start_time
limit 3;
-- where语句的使用也与select相似
```

#### 2. 删除所有记录并重置表结构，让主键重新从0递增(truncate)

```sql
truncate table exam_record
```

#### 3. drop、delete和truncate的区别

1.DROP TABLE　清除数据并且销毁表，是一种数据库定义语言(DDL Data Definition Language), 执行后不能撤销，被删除表格的关系，索引，权限等等都会被永久删除。

2.TRUNCATE TABLE　只清除数据，保留表结构，列，权限，索引，视图，关系等等，相当于清零数据，是一种数据库定义语言(DDL Data Definition Language)，执行后不能撤销。

3.DELETE FROM TABLE　删除（符合某些条件的）数据，是一种数据操纵语言(DML Data Manipulation Language)，执行后可以撤销。

运行速度一般DROP最快，DELETE最慢，但是DELETE最安全。

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

| 函数                                                         | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| LENGTH()                                                     | 返回字符串的长度                                             |
| UPPER() / LOWER()                                            | 全部大写或小写                                               |
| LTRIM() / RTRIM() / TRIM()                                   | 去除左空格/右空格/全部空格                                   |
| LEFT(“string”,左侧多少个字符)                                | left("string",3)<br />--- 输出 str                           |
| RIGHT("string",右侧多少个字符)                               | right("string",3)<br />--- 输出 ing                          |
| SUBSTRING("string",起始字符位置，结束字符位置[可选])         | substring("string",2,5)<br />--- 输出trin                    |
| LOCATE(子串,字符串)                                          | 返回子串第一次出现的位置                                     |
| REPLACE(字符串，内容，替换内容)                              |                                                              |
| CONCAT(连接多个字符串)                                       | concat(str1,str2,str3)                                       |
| GROUP_CONCAT([distinct] 连接的字符串 [order by 字段 asc/desc] [separator '分隔符']) | 在使用了group by后可以使用，可以将组内的一些字段各行的值组合起来。如: group_concat(distinct date,':',tag)，会返回{2021-01-02:SQL;2021-01-12:JAVA} |
| substring_index(str,delim,count)                             | str:要处理的字符串 delim:分隔符 count:计数                   |

#### 日期函数 

| 函数                                                         | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| NOW()                                                        | 返回类似：2022-01-21 16:40:24                                |
| CURDATE()                                                    | 返回类似：2022-01-21                                         |
| CURTIME()                                                    | 返回类似：16:40:24                                           |
| YEAR(NOW())                                                  | 返回类似：2022                                               |
| MONTH(NOW())                                                 | 返回类似：1                                                  |
| 同类的还有DAY()、HOUR()、MINUTE()、SECOND()                  | 天、时、分、秒                                               |
| LAST_DAY('2021-01-27')                                       | 获取本月最后一天，返回2021-01-31                             |
| DATE_ADD('2022-01-02',interval 1 day)                        | 增加时间 后面可以是interval 1 month/ 2 hour 之类的           |
| DATEDIFF(end_date,start_date)                                | 日期差，不会显示时分秒                                       |
| TIMEDIFF(end_time,start_time)                                | 时间差，显示时分秒                                           |
| TIMESTAMPDIFF(day/minute/hour/second,start_time,end_time)<br/>--- 注意下<font color="red">这里是先start_time再end_time，和上面两个反着的</font> | select TIMESTAMPDIFF(HOUR, '2022-01-18 09:00:00', '2022-01-20 10:00:00');<br/>--- 相差49个小时 |
| DATA_FORMAT(date,"%Y%m%d")                                   | 格式化时间 DATA_FORMAT('2021-01-27',"%Y%m") 可得202101       |



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



### EXPLAIN分析SQL语句

#### 1. 介绍

【参考：https://www.cnblogs.com/deverz/p/11066043.html】

#### 2. 简单使用

```sql
explain select * from user_profile where device_id = 2138
-- 查询结果是对上面的语句进行的分析
```

![image-20220126101634317](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220126101634317.png)

可以看到type=all，是使用了全表扫描，rows=7，表示扫描了7行。

这里现在对device_id建立索引，再重新进行一次查询:

```sql
-- 创建索引
create index idx_device_id on user_profile(device_id);
-- 分析语句
explain select * from user_profile where device_id = 2138;
```

![image-20220126102636851](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220126102636851.png)

可以看到，在使用了device_id作为索引后，type变为了ref，表示采用了索引查询，rows=1，表示只扫描了一行，比之前要扫描7行，要很多。



### 索引

!> 索引的目的就是为了能够加快查询，因此索引是要基于查询创建的，而不应该随意地去创建索引，否则会导致资源的浪费与消耗的白白增加。<br />一般记录比较少的表是没有必要创建索引的，全局查询是可以的。创建索引会增加数据库的大小，而且在对表进行增加、删除、修改的时候都需要更新索引，因此索引的选择一定要合适，否则达不到提高性能的效果。<br/>注：mysql采用innodb数据库引擎下，会默认将主键创建为聚类索引(cluster index)，且聚类索引只能有一个。

#### 1. 索引的创建

##### 方法一(create)

```sql
-- 普通索引
create index idx_uid on users(uid)
-- 唯一索引
create unique index unq_idx_uid on users(uid)
-- 全文索引(全文索引可以类比搜索引擎搜索内容，后面具体讲)
create fulltext index full_idx_passage_abstract on blogs(passage,abstract)
```

##### 方法二(alter)

```sql
-- 普通索引
alter table users
add index idx_uid(uid);
-- 唯一索引
alter table users
add unique index unq_idx_uid(uid);
-- 全文索引
alter table blogs
add fulltext index full_idx_passage_abstract(passage,absract);
```

##### 关于全文索引的补充

参考：https://www.bilibili.com/video/BV1UE41147KC?p=140&spm_id_from=pageDriver

全文索引一般适用于比较长的字段，如文章内容，需要查询与搜索字符串匹配的文章。例如想要搜索 “python 数据库连接"，那么我们期望的肯定不是说要完全有连着的这样的一个字符串，而是说要找到有python和字符串连接这样的内容的文章，就类似百度搜索一样，全文搜索就起着类似的作用。

```sql
-- 创建全文搜索(从文章和摘要中找到想要的内容，所以为文章和摘要字段设置全文索引)
create fulltext index full_idx_passage_abstract(passage,abstract)

-- 字符串匹配(默认模式)
select *,match(passage,abstract) against('python 数据库连接') as score from blogs -- 可以查询这个表达式，会返回结果的相关性
where match(passage,abstract) against('python 数据库连接') -- 注意：这里必须要把所有的全文索引字段写上，否则会报错

-- 字符串匹配(布尔模式)
select * from blogs
where match(passage,abstract) against('python 数据库连接' in boolean mode) -- 表示使用了布尔模式
/**
在bool模式下，可以通过 against('+python -数据库连接' in boolean mode) 表示必须要有python，必须排除数据库连接
against('python 数据库连接 -java' in boolean mode) 表示有python或者数据库连接，不能有java
against('"python 数据库连接"' in boolean mode) 单引号里面加双引号，表示要完全匹配这个字符串，类似where中的 = 
**/
```

#### 2. 查看索引

```sql
-- 方法1
show index from examination_info
-- 方法2
show indexes in examination_info
```

![image-20220126102417758](https://gitee.com/y255413580/img/raw/master/noteimg/image-20220126102417758.png)

其中，collation表示的是采用的排序方式 A即asc，D即desc，index_type为BTREE，这里应该表示的其实是说索引的数据结构为B+树。



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



