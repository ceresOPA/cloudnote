# SQL

> 学习资源：[自学SQL网](http://xuesql.cn/)



## SQL条件查询

### 1. 使用WHERE语句筛选数字类型的属性

> 把不太熟悉的sql条件查询的语句记一下，之前除了=、!=这种常用的，很少用到下面的四种

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220118220927157.png" alt="image-20220118220927157" style="zoom: 50%;" />

### 2. 字符串相关筛选

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220118223539058.png" alt="image-20220118223539058" style="zoom:50%;" />

?> 关于字符匹配中，可能存在的大小写问题，可以通过将字符串先全部转换为大写或小写，然后再去匹配：

```sql
select * from movies where upper(title) like "%STRING%";
select * from movies where lower(title) like "%string%";
```



## SQL筛选与排序

### 1. distinct 与 group by

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

### 2. 排序(order by)

?> order by col_name(asc/desc), 根据某一列进行排序<br />数值类型的列：直接按照数值大小排序（asc：9 8 7... ；desc：1 2 3...）<br />字符类型的列：按照字母表的顺序进行排序

```sql
select * from movies order by id asc; --默认是asc升序
```

### 3. 筛选(limit  ; offset)

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



## 关键字NULL的使用

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

