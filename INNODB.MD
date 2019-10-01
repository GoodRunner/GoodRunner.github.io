#### innodb引擎内幕

##### 检查引擎使用情况

`show engine innodb status;`

##### 慢查询

`mysqldump **slow.log`

在mysql库中有一个`slow_log`表记录了慢查询

##### 二进制日志

`show variables like 'datadir'` 查看二进制日志(bin_log)路径

##### mysql分区表

分区功能并不是在存储引擎完成的，因此并不只有INNODB支持分区，常见的存储引擎，
不论创建何种类型的分区，如果表中存在主键或唯一索引时，分区列必须是索引的一个组成部分。

**RANGE分区**
> 数据基于属于一个给定的连续区间的列值放入分区。可以让数据按照ID分区和日期分区
```sql
alter table tname
	PARTITION by RANGE(id)(
	PARTITION p0 values less than(20),
	PARTITION p1 values less than(40),
	PARTITION pm VALUES LESS THAN(MAXVALUE)
);

```

```sql
alter table tname
	PARTITION by RANGE(YEAR(date))(
	PARTITION p2008 values less than(2009),
	PARTITION pm VALUES LESS THAN(MAXVALUE)
);
```

创建日期分区表的好处
1. 是便于对表的管理，如需要删除2008年的数据就只需要删除相应的分区就行。 `alter table tname drop partition p2008`
2. 加速某些查询 ` select * from tname where date > '2008-01-01' and date < '2008-12-28' `;
3. 需要注意的是 RANGE分区的查询，优化器只会对`YEAR()`、`TO_DAYS()`、`TO_SECONDS()`、`UNIX_TIMESTAMP()`这类函数进行优化选择。


<strong>LIST分区</strong>
> list分区和range分区类似，只是list分区面向的是离散的值。
```sql
alter table tname
	PARTITION by LIST(id)(
	PARTITION p0 values in(1,2,3),
	PARTITION p1 values in (4,5,6)
);
```

**HASH分区**
> 根据用户自定义的表达式的返回值来进行分区，返回值不能是负数。哈希分区是的目的是将数据均匀的分不到预先定义的各个分区中，保证各个分区中的数据数量大致都是一样的
```sql
create table tname (
	id int PRIMARY KEY AUTO_INCREMENT,
	name varchar(20)
	)
	PARTITION by HASH (id)
	partitions 3;
```


**KEY分区**
> 根据mysql数据库提供的哈希函数来进行分区。

