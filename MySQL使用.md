##  MySQL使用

####  数据库操作

#####  	创建数据库

```mysql
create database db1(数据库名);
```

#####  	切换到数据库

```mysql
use db1;
```

#####  	删除数据库

```mysql
drop database db1(数据库名);
```

---

####  数据表操作

#####  	创建数据表

```mysql
create table 表名(
id int,
name varchar(20),
gender varchar(10),
...
);
```

- 数据类型
   - 数值：

     ![p3](D:\MYGITS\camp-daily\p3.png)

  - 时间&日期：
    ![p4](D:\MYGITS\camp-daily\p4.png)

  - 字符串：
    ![p5](D:\MYGITS\camp-daily\p5.png)

  - 二进制类型：用于存放图片、pdf等文件

    ![p6](D:\MYGITS\camp-daily\p6.png)

  #####  查看字段信息

```mysql
desc 表名;
```

#####  	删除数据表

```mysql
drop table 表名;
```

##### 	约束数据表

 	为防止插入错误数据，可对表中字段进行限制。

![p7](D:\MYGITS\camp-daily\p7.png)

- ```mysql
  字段名 数据类型 primary key;
  #在表中是唯一的且其值不能为空
  ```

- ```mysql
  字段名 数据类型 not null;
  ```

- ```mysql
  字段名 数据类型 default 默认值；
  ```

- ```mysql
  字段名 数据类型 unique;
  #表中字段的值不能重复出现
  ```

  #####  插入数据

  ```mysql
  insert into 表名（字段名1,字段名2,...) values (值 1,值 2,...),...;
  ```

---

####  查询数据操作

```mysql
select 字段名1,字段名2,... from 表名1,... [where][limit][offset];
```

- 字段名若传入‘*’，则为查询全部字段。

- `limit`表示结果最多数量

- `offset`标识开始查询时的偏移量，默认为0

  #####  where

  可理解为条件语句(if)，其后可以传入约束条件，条件之间可用`and`与`or`连接

  ```mysql
  where condition1 [and [or]] condition2.....
  ```

  一些约束条件关键词：

  #####  1. in

  in关键字用于判断某个字段的值是否在指定集合中。如果字段的值恰好		在指定的集合中，则将字段所在的记录将査询出来。

  #####  2. between and

  between and用于判断某个字段的值是否在指定的范围之内。如果字段的值在指定范围内，则将所在的记录将查询出来

  #####  3. like

  like关键字可以判断两个字符串是否相匹配。具体分为3种情况。

  - 普通字符串

  - 含有%的通配字符串

    > %用于匹配任意长度的字符串。例如，字符串“a%”匹配以字符a开始任意长度的字符串

  - 含有_的通配字符串

    > 下划线通配符只匹配单个字符，如果要匹配多个字符，需要连续使用多个下划线通配符。例如，字符串“ab_”匹配以字符串“ab”开始长度为3的字符串，如abc、abp等等；字符串“a__d”匹配在字符“a”和“d”之间包含两个字符的字符串，如"abcd"、"atud"等等。

---

####  alter命令

#####  修改表名

```mysql
 alter table 原表名 rename to 新表名;
```

#####  删除字段

```mysql
alter table 表名 drop 字段名;
```

#####  添加字段

```mysql
alter table 表名 add i int [first]/[after ...];
```

#####  修改字段类型、名称

- modify：

  ```mysql
  alter table 表名 modify 字段名 新类型;
  ```

- change:

  ```mysql
  alter table 表名 change 原字段名 新字段名 新类型;
  ```

#####  修改字段默认值

```mysql
alter table 表名 alter 字段名 set default 新默认值;
```

```my
alter table 表名 alter 字段名 drop default;
```

