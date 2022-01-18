# SQL条件查询

1. 使用WHERE语句筛选数字类型的属性

> 把不太熟悉的sql条件查询的语句记一下，之前除了=、!=这种常用的，很少用到下面的四种

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220118220927157.png" alt="image-20220118220927157" style="zoom: 50%;" />

2. 字符串相关筛选

<img src="https://gitee.com/y255413580/img/raw/master/noteimg/image-20220118223539058.png" alt="image-20220118223539058" style="zoom:50%;" />

?> 关于字符匹配中，可能存在的大小写问题，可以通过将字符串先全部转换为大写或小写，然后再去匹配：

```sql
select * from movies where upper(title) like "%STRING%"
select * from movies where lower(title) like "%string%"
```

