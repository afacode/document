# mysql

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

### 库操作
```js
show databases;
create database db1 default charset utf8 COLLATE utf8_general_ci;
```


### 表操作
* 新增表 
```js 
create table if not exists `tab1` ( 
	`nid` int(11) NOT NULL auto_increment, 
	`name` varchar(255) DEFAULT zhangyanlin, 
	`email` varchar(255), 
	PRIMARY	KEY (`nid`) 
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
```js
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
* 修改表名 `rename table 旧表名 to 新表名;`
* 修改表选项 `alter table + 表名 + 表选项[=] + 值;`
* 修改表中的字段，新增、修改、重命名和删除
	* 新增字段 `alter table + 表名 + add + [column] + 字段名 + 数据类型 + [列属性][位置];`
		* `alter table student add column id int first;`
		* 位置表示此字段存储的位置，分为first(第一个位置)和after + 字段名(指定的字段后，默认为最后一个位置)
	* 修改字段 `alter table + 表名 + modify + 字段名 + 数据类型 + [列属性][位置];`
		* `alter table student modify name char(10) after id;`
		* 位置表示此字段存储的位置，分为first(第一个位置)和after + 字段名(指定的字段后，默认为最后一个位置)
	* 重命名字段 `alter table + 表名 + change + 旧字段名 + 新字段名 + 数据类型 + [列属性][位置];` 
		* `alter table student change grade class varchar(10);`
		* 位置表示此字段存储的位置，分为first(第一个位置)和after + 字段名(指定的字段后，默认为最后一个位置)）
	* 删除字段 `alter table + 表名 + drop+ 字段名;`
		* `alter table student drop age;` 如果表中已经存在数据，那么删除该字段会清空该字段的所有数据，而且不可逆，慎用
* 删除表 `drop table table_name, tablename2;` 表结构
 * `delete from table_name;` 表数据 


### 数据操作

* 新增数据 insert
```js
INSERT INTO table_name 
	(field1, field2,...fieldN)
  VALUES
  ( value1, value2,...valueN )
```
`insert into test values('charies',18,'3.1');`
* 查询数据 select
```js
select * from + 表名 + [where 条件];
select * from table_name; # 全部字段
select all * from table_name;
select distinct * from table_name; table_name 
# 去重,将查出来的结果中所有字段都相同的记录去除
```
* 更新数据 update 
```js
UPDATE table_name SET 
	field1=new-value1, field2=new-value2
	[WHERE Clause]
affected的数量大于1时 才是真正的更新成功
```
* delete
```js
DELETE FROM table_name [WHERE Clause]
delete from test where grade = '3.1';
我们也可以用drop来实现删除操作，不过与delete相比，drop的威力更强,
其在执行删除操作的时候，不仅会删除数据,
还会删除定义并释放存储空间;
而delete在执行删除操作的时候,仅会删除数据,并不会删除定义和释放存储空间
```
### 中文数据问题
* 通过 MySQL 数据库的客户端向服务器插入中文数据的时候，有可能失败，原因则可能是客户端和服务器的字符集设置不同导致的
	* 客户端的字符集为gbk，则一个中文字符，对应两个字节
	* 服务器的字符集为utf8，则一个中文字符，对应三个字节
* 查看服务器识别的全部字符集 `show character set;`

## where


## like


## limit


## asc, desc



## group by
## 





















