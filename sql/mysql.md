## mysql
```sql
select * from table_name; # 全部字段
select all * from table_name;
select distinct * from table_name; table_name # 去重，将查出来的结果中所有字段都相同的记录去除
```
group by order by limit

```js
show databases;
create database db1 default charset utf8 COLLATE utf8_general_ci;
use db1;
show tables;
create table `tab1` ( `nid` int(11) NOT NULL auto_increment, `name` varchar(255) DEFAULT zhangyanlin, `email` varchar(255), PRIMARY KEY (`nid`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

drop table table_name; # 删除表
delete from table_name; 
truncate table table_name

```
## create
` create database database_name ` <br>
` create database database_name default charset utf8 COLLATE utf8_general_ci; ` <br>
` CREATE TABLE table_name (column_name column_type) ` <br>
#### demo
```js
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
## drop
` drop database database_name ` <br>
` drop table table_name ` <br>
## 数据类型
> 数值类型
* tinyint
* smallint
* mediumint
* int / integer
* bigint
* float
* double
> 日期和时间类型
* date YYYY-MM-DD 日期值
* time HH:MM:SS 时间值或持续时间
* year YYYY
* datetime YYYY-MM-DD HH:MM:SS 混合日期和时间值
* timestamp YYYYMMDD HHMMSS 混合日期和时间值，时间戳
> 字符串类型
* char 0-255字节	 定长字符串
* varchar 0-65535 字节	变长字符串
* tinyblob 0-255字节	不超过 255 个字符的二进制字符串
* tinytext 
* text
* longtext

## insert
```js
INSERT INTO table_name 
	( field1, field2,...fieldN )
  VALUES
  ( value1, value2,...valueN )
```

## delete
```js
DELETE FROM table_name [WHERE Clause]
```

## update
```js
UPDATE table_name SET 
	field1=new-value1, field2=new-value2
	[WHERE Clause]
```

## select


## where


## like


## limit


## asc, desc



## group by
## 





















