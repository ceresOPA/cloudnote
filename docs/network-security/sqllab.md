# sqli-lab学习

> 参考资料：https://blog.csdn.net/Yb_140/article/details/123223306

?> 本人也是网安入门小白，所以基础知识几乎为0，感觉sql注入挺好玩的，故尝试学习。下面的过程均为个人实践流程，仅作参考，还请各位大佬多多指教！

## 一、sqli-lab安装

sqli-lab安装的前置工作需要先安装好phpstudy或其他的php集成开发环境，这里就不过多介绍。

sqli-lab的下载的地址为：https://github.com/Audi-1/sqli-labs

下载完成后，只需要将压缩包解压放到网站的WWW目录下就可以了，如果使用的phpstudy的话，网站的根目录可以通过下面的方式进行查看。

![image-20220526103658932](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220526103658932.png)

接下来就是对于数据库的配置，进入到sqli-labs的文件夹下，再进入到sql-connections文件夹下，可以看到db-creds.inc这样一个配置文件，选择进行编辑。

![image-20220526104701848](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220526104701848.png)

这里只需要填写好自己mysql数据库的账号和密码就可以了。

![image-20220526104930416](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220526104930416.png)

然后通过http://localhost/sqli-labs访问即可，可以在这个界面的左上角看到有Setup/reset Database for labs，点击即可自动导入数据库，完成sqli-labs的安装。

![image-20220526105110845](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220526105110845.png)

## 二、实战

### Ⅰ Basic Challenge

#### 1. Less-1

![image-20220526105322086](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220526105322086.png)

一上来，说让传一个id这样的参数，这里就先直接给他传了一个id=0，执行完成后发现是没有任何回写的。

![image-20220526105546854](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220526105546854.png)

说明就应该不存在id=0这样的数据，这里应该可以选择疯狂遍历的试吧，但也还有更有趣的方法来破解。

这里可以猜想执行的sql语句为 select * from table_name where id = 0，那如果在条件这里增加一个 or 1=1，那是不是就无论输入什么样的id，都会查出数据呢？所以，按照这个思路，可以让id = 0 or 1=1，这里为避免后面还有其他的一些筛选条件，可以使用 --+，来将后面的给注释掉，这里--就是注释符，+只是用来替代空格的，因为如果直接使用空格有可能传过去后会自动剔除。

![image-20220527091243844](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220527091243844.png)

发现还是没有登陆成功，所以可以考虑是否是使用了字符串类型的id，select * from table_name where id='0' 这样的方式，因此，可以使用单引号（单引号或双引号，依照具体情况来）来提前闭合，这样就不会导致把 or 1=1--+这些内容全放在字符串里面去了。

![image-20220527092005623](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220527092005623.png)

这样就登陆成功了，这里我们即使不知道id是多少也没有关系，因为 or 1=1是永远为真的，甚至可以使用 limit 0,1 查询出所有的name和password。

![image-20220527092158555](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220527092158555.png)

能够实现回显后，接下来就可以做更多的事情了，为能够进行联合查询，必然就得知道当前查询的这个的表有多少个字段，这样才能够匹配的上，所以这里可以借助order by来判断。

因为order by 恰好有两个特点可以达成这个目的：

1. 即使在不知道字段名的情况下，order by 还能够通过 order by 1 、order by 2 这样的方式，来按照第一个字段排序、按照第二个字段来排序
2. 当例如 order by 4 这样执行下去后，如果字段存在则会正常排序并回显，如果字段不存在了，则会报错

因此，通过这样的方式一个个试下去，就可以知道字段个数了。

?> 这里发现也没必要用order by 1 这样一个个试，可以直接 order by 1,2,3,4,5,6 这样，按sql语句的含义就是，按1,2,3,4,5,6优先级从高到低进行排序，总之就是当他遇到不存在的字段的时候，就会停下来的。

![image-20220527093008073](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220527093008073.png)

可以看到，这里到 column 4就停下来了，因此，可以知道这个表有3个字段。ok，这样就可以进行我们的联合注入了。

![image-20220527093152874](https://cdn.jsdelivr.net/gh/ceresopa/img/imgimage-20220527093152874.png)

可以看到已经成功地使用了union，将2和3显示了上去，这样我们就可以显示出我们自己想要显示出来的内容了。这里可以看到还加上了limit 13,1 因为union增加的这条记录是放在最后的，然后每次只会显示出第一条记录，因此通过limit来指定到了最后一条。



### Ⅱ Advanced Injections

### Ⅲ Stacked Injections

### Ⅳ Challenge

