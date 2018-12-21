# mysql

---
推荐  [维C果糖](https://blog.csdn.net/qq_35246620/article/details/70823903)

## 数据类型
> 数值类型

---
整数型
---
* tinyint
* smallint
* mediumint
* int / integer
* bigint

---
小数型
---
> 浮点型：小数点浮动,精度有限,容易丢失精度
* float
* double
```js
create table my_decimal(
    f1 float(10,2),
    d1 decimal(10,2)
)charset utf8;
```
> 定点型：小数点固定,精度固定,不会丢失精度
```js
create table my_decimal(
    f1 float(10,2),
    d1 decimal(10,2)
)charset utf8;
```

> 日期和时间类型

* date YYYY-MM-DD 日期值
* time HH:MM:SS 时间值或持续时间
* year YYYY 两种格式,分别为year(2)和year(4), 默认YYYY
* datetime YYYY-MM-DD HH:MM:SS 混合日期和时间值
* timestamp YYYYMMDD HHMMSS 混合日期和时间值,时间戳

> 字符串类型

* char 0-255字节	 定长字符串
* varchar 0-65535 字节	变长字符串 varchar(10)存储 10 个汉字,在 UTF8 环境下,需要 10*3+1=31 个字节
* tinyblob 0-255字节	不超过 255 个字符的二进制字符串
* tinytext 
* text
* longtext
* blob 存储二进制数据(其实际上都是存储路径),通常不用
* enum 枚举字符串 eg `gender enum('男','女','保密')` 枚举字符串有一个好处 规范数据格式,插入表中的数据只能是事先定义好的某个数据 节省存储空间(枚举数据通常都有一个别名),枚举实际上存储的是数值而不是字符串本身
* set 集合字符串 实际存储的是数值而不是字符串 `hobby set('音乐','电影','旅行','美食','摄影','运动','宠物')` 集合字符串中每一个元素都对应一个二进制位,其中被选中的为1,未选中的为0,最后在反过来,这个二进制数对应的十进制数即为其数据库中实际存储的是数值

> 列属性
---
真正约束字段的是数据类型,但是数据类型的约束比较单一,因此就需要额外的一些约束来保证数据的有效性,这就是列属性
---
> 列属性有很多,例如：null、not null、default、primary key、unique key、auto_increment和comment等
* 空属性 null | not null 数据库默认的字段基本都为空 null
* 列描述 comment 是专门用来描述字段的, 备注
* 默认值 default 
* 自动增长 auto_increment
	* 当对应的字段,不给值,或者是默认值,或者是null的时候,就会自动的被系统触发,系统会从当前字段中取已有的最大值再进行+1操作,得到新的字段值
	* 任何字段要做自增长,前提其本身必须是一个索引,即key栏有值
	* 自增长字段必须是数字(整型);
	* 每张表最多有一个自增长字段
	* `id int primary key auto_increment,`
	* 修改自增长
		* 自增长如果是涉及到字段改变,就必须先删除自增长,然后再增加自增长,因为每张表只能有一个自增长字段
		* 如果修改当前自增长字段已经存在的值,则只能修改比当前已有自增长字段中的最大值更大,不能更小,因为更小不生效
		* eg: `alter table my_auto auto_increment = 2;`
	* 删除自增长
		* 自增长是字段的一个属性,因此可以通过modify来进行修改.想要删除自增长的话,只需要保证字段没有auto_increment即可
		* `alter table + 表名 + modify + 字段 + 类型;`
		* `alter table my_auto modify id int`
* 唯一键 unique unique key
	* 每张表往往有多个字段需要具有唯一性,数据不能重复,但是在每张表中,只能有一个主键,因此唯一键就是用来解决表中多个字段需要具有唯一性的问题
	* 唯一键的本质与主键差不多,唯一键默认的允许字段为空,而且可以多个字段为空,因此空字段不参与唯一性的比较
	* 字段后面直接添加 `number char(10) unique comment '学号',`
	* 所有字段之后添加
	```js
	create table my_unique2(
    number char(10) not null,
    name varchar(20) not null,
    unique key(number)  
	)charset utf8; 
	```
	* 更新唯一键 & 删除唯一键 `alter table + 表名 + drop index + 索引名字;`
* 主键 primary key 表中主要的键,每张表只能有一个字段(复合主键,可以多个字段)使用此属性,用来唯一的约束该字段里面的数据,表中对应字段的数据是不重复的,即保证唯一性
	* 增加主键
		1. 建表时,直接在字段添加 `number char(10) primary key comment '学号'` 优点是清晰明了,缺点则是只能使用一个字段作为主键
		2. 建表时,所有的字段之后,使用`primary key`(主键字段列表)来创建主键(如果有多个字段作为主键,称为复合主键)
		```js
		create table my_pri2(
			number char(10) not null comment '学号',
			course char(10) not null comment '课程编号',
			score tinyint unsigned default 60,
			-- 增加主键限制,学号和课程编号应该是对应的,具有唯一性
			primary key(number,course)
		)charset utf8;
		```
		3. 表建完之后,额外追加主键,可以直接追加主键,也可以通过修改表字段的属性追加主键
			* `alter table my_pri3 modify course char(10) primary key comment '课程编号'; -- 不建议使用`
			* `alter table my_pri3 add primary key(course); -- 推荐使用`
	* 主键约束 
		* 主键对应的字符中的数据不允许重复,如果重复,则数据操作(主要是增和改)失败
	* 更新主键 & 删除主键
		* 删除主键`alter table + 表名 + drop primary key; eg alter table my_pri3 drop primary key;`
		* 增加主键的方法, eg: `alter table my_pri3 add primary key(course);`

* 对对对






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
* 修改表中的字段,新增、修改、重命名和删除
	* 新增字段 `alter table + 表名 + add + [column] + 字段名 + 数据类型 + [列属性][位置];`
		* `alter table student add column id int first;`
		* 位置表示此字段存储的位置,分为first(第一个位置)和after + 字段名(指定的字段后,默认为最后一个位置)
	* 修改字段 `alter table + 表名 + modify + 字段名 + 数据类型 + [列属性][位置];`
		* `alter table student modify name char(10) after id;`
		* 位置表示此字段存储的位置,分为first(第一个位置)和after + 字段名(指定的字段后,默认为最后一个位置)
	* 重命名字段 `alter table + 表名 + change + 旧字段名 + 新字段名 + 数据类型 + [列属性][位置];` 
		* `alter table student change grade class varchar(10);`
		* 位置表示此字段存储的位置,分为first(第一个位置)和after + 字段名(指定的字段后,默认为最后一个位置))
	* 删除字段 `alter table + 表名 + drop+ 字段名;`
		* `alter table student drop age;` 如果表中已经存在数据,那么删除该字段会清空该字段的所有数据,而且不可逆,慎用
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
> `insert into test values('charies',18,'3.1');`
* 查询数据 select
	* `select + 字段列表/* + from + 表名 + [where 条件];`
	* `select + [select 选项] + 字段列表[字段别名]/* + from + 数据源 + [where 条件] + [1] + [2] + [3];`
		* group by
		* order by
		* limit
	* 字段别名 `字段名 + [as] + 别名;` `name as 姓名,`
	* 子查询 `select * from + (select * from + 表名) + [as] + 别名;`
```js
select * from table_name; # 全部字段
select all * from table_name;
select distinct * from table_name; 
select * from + 表名1,表名2...;  # 多表数据源
# distinct 去重,将查出来的结果中所有字段都相同的记录去除
```

---
高级
---
> where

* 判断条件
	* 比较运算符：>、<、>=、<=、<>、=、like、between and、in和not in；
	* 逻辑运算符：&&、||、和!
	```js
	-- 查询表 student 中 id 为 2、3、5 的记录
	select * from student where id = 2 || id = 3 || id = 5;
	select * from student where id in (2,3,5);
	
	-- 查询表 student 中 id 在 2 和 5 之间的记录 包含端点值
	-- and 前面的数值必须大于等于 and 后面的数值,否则会出现空判断
	select * from student where id between 2 and 5;
	```
> group by 根据表中的某个字段进行分组,即将含有相同字段值的记录放在一组,不同的放在不同组
--- 分组的目的是为了(按分组字段)统计数据,并不是为了单纯的进行分组而分组 ---
* cout()：统计分组后,每组的总记录数；个数
* max()：统计每组中的最大值；
* min()：统计每组中的最小值；
* avg()：统计每组中的平均值；
* sum()：统计每组中的数据总和
	* -- 将表 student 中的数据按字段 sex 进行分组,并进行统计
	* `select sex,count(*),max(age),min(age),avg(age),sum(age) from student group by sex asc;` # desc

> having
---
与where子句一样,都是进行条件判断的,但是where是针对磁盘数据进行判断,数据进入内存之后,会进行分组操作,分组结果就需要having来处理.思考可知,having能做where能做的几乎所有事情,但是where却不能做having能做的很多事情
---
* 分组统计的结果或者说统计函数只有having能够使用
```js
-- 求出表 student 中所有班级人数大于等于 2 的班级
select grade,count(*) from student group by grade having count(*) >= 2;
select grade,count(*) from student where count(*) >= 2 group by grade;
```
* having能够使用字段别名,where则不能
```js
-- 求出表 student 中所有班级人数大于等于 2 的班级
select grade,count(*) as total from student group by grade having total >= 2;
select grade,count(*) as total from student where total >= 2 group by grade;
```
---
where是从磁盘读取数据,而磁盘中数据的名字只能是字段名,别名是数据(字段)进入到内存后才产生的.
值得注意的是,在上述 SQL 语句中咱们使用了字段别名,这在无意中就优化了 SQL 并提高了效率,因为少了一次统计函数的计算
---
> order by 根据某个字段进行升序或者降序排序,依赖校对集
* asc为默认值; desc为降序
```js
// -- 将表 student 中的数据按年龄 age 进行排序
select * from student order by age;
// 将表 student 中的数据先按年龄 age 升序排序,再按班级 grade 降序排序
select * from student order by age,grade desc;
```
> limit 是一种限制结果的语句,通常来限制结果的数量
* `limit + [offset] + length;` offset为起始值；length为长度
* 只用来限制长度(数据量)
`select * from student limit 3; // 查询表 student 中的 3 条记录`
* 限制起始值,限制长度(数据量)
`select * from student limit 2,2;` 从第二个开始去2个
* 主要用来实现数据的分页,目的是为用户节省时间,提高服务器的响应效率,减少资源的浪费
	* length: 表示每页的数据量,基本不变
	* offset: 每页的起始值 公式为offset=(页码-1)*length 

> 连接查询 将多张表(大于等于2张表)按照某个指定的条件进行数据的拼接,其最终结果记录数可能有变化,但字段数一定会增加
* 外连接 left join || right join `左表 + left\right + join + 右表 + on + 左表.字段 = 右表.字段;`
	* left join 以左表为主表, 左表全部显示
	* right join 以右表为主表
* 自然连接 nature join 自然连接并不常用
	*自动匹配连接条件,系统以两表中同名字段作为匹配条件,如果两表有多个同名字段,那就都作为匹配条件.
	* 自然内连接 `左表 + nature + join + 右表;`
	```js
	-- 将表 student 与 class 进行自然内连接
	select * from student natural join class;
	-- 将表 student 与 class 进行内连接,连接条件为 id 和 grade
	select * from student inner join class on student.id = class.id and student.grade = class.grade;
	```
	* 自然外连接 `左表 + nature + left/right + join + 右表;`
	```js
	-- 将表 student 与 class 进行自然左外连接
	select * from student natural left join class;
	-- 将表 student 与 class 进行自然右外连接
	select * from student natural right join class;
	```
* 内连接 inner join 交集, 且的关系
	* 从左表中取出每一条记录,和右表中的所有记录进行匹配,并且仅当某个条件在左表和右表中的值相同时,结果才会保留,否则不保留
	```js
	-- 将表 student 与 class 进行内连接
	select * from student inner join class on student.grade = class.grade;
	select * from student join class on student.grade = class.grade;
	```
* 交叉连接 cross join
	* 从一张表中循环取出每一条记录,每条记录都去另外一张表进行匹配,匹配的结果都保留(没有条件匹配),而连接本身的字段会增加,最终形成的结果为笛卡尔积形式
	* `左表 cross join 右边;`
	* 笛卡尔积形式(交叉连接和多表查询)的结果并没有什么实际意义,应该尽量避免,其存在的价值就是保证连接这种结构的完整性


* 更新数据 update 
> `update + 表名 + set + 字段 = 值 + [where 条件] + [limit 更新数量];`
```js
UPDATE table_name SET 
	field1=new-value1, field2=new-value2
	[WHERE Clause]
affected的数量大于1时 才是真正的更新成功
```
* delete
```js
delete + from + 表名 + [where 条件] + [limit 删除数量];
delete from test where grade = '3.1';
我们也可以用drop来实现删除操作,不过与delete相比,drop的威力更强,
其在执行删除操作的时候,不仅会删除数据,
还会删除定义并释放存储空间;
而delete在执行删除操作的时候,仅会删除数据,并不会删除定义和释放存储空间
```
### 中文数据问题
* 通过 MySQL 数据库的客户端向服务器插入中文数据的时候,有可能失败,原因则可能是客户端和服务器的字符集设置不同导致的
	* 客户端的字符集为gbk,则一个中文字符,对应两个字节
	* 服务器的字符集为utf8,则一个中文字符,对应三个字节
* 查看服务器识别的全部字符集 `show character set;`


### 校对集collate 数据的比较方式
校对集,共有三种,分别为
* _bin：binary,二进制比较,区分大小写
* _cs：case sensitive,大小写敏感,区分大小写
* _ci：case insensitive,大小写不敏感,不区分大小写
`show collation;`


#### 索引
***
系统根据某种算法,将已有的数据(未来可能新增的数据),单独建立一个文件,这个文件能够实现快速匹配数据,并且能够快速的找到对应的记录,几乎所有的索引都是建立在字段之上的
***
* 提升查询数据的效率
* 约束数据的有效性
* MySQL中提供了多种索引
	* primary key
	* unique key
	* 全文索引 fulltext index
	* 普通索引 index


### 外键 foreign key
---
即不在自己表中的键.如果一张表中有一个非主键的字段指向另外一张表的主键,那么将该字段称之为外键.
每张表中,可以有多个外键
---
* 新增外键
	* 在创建表的时候,增加外键
		* `foreign key(外键字段) + references + 外部表名(主键字段);`
		* 在所有的字段设计完之后 `foreign key(c_id) references class(id)`
	* 在创建表之后,增加外键
		* `alter table + 表名 + add[constraint + 外键名字] + foreign key(外键字段) + references + 外部表名(主键字段);`
		* `alter table my_foreign2 add constraint test_foreign foreign key(c_id) references class(id);`
* 修改外键 & 删除外键  外键不能修改,只能先删除后增加.
	* `alter table + 表名 + drop foreign key + 外键名字;`
	* `alter table my_foreign1 drop foreign key my_foreign1_ibfk_1;` 删除外键



### 关系
### 范式
---
是为了解决数据的存储和优化问题
---
### 联合查询 union
将多次查询(多条select语句)的结果,在字段数相同的情况下,在记录的层次上进行拼接
* `select 语句1 + union + [union选项] + select 语句2 + ...;`
* union 选项
	* all 无论重复与否,保留所有记录
	* distinct 表示去重,为默认选项
	```js
	select * from class union distinct select * from class;
	select * from class union all select * from class;
	```
* 意义
	* 查询同一张表,例如查询学生信息,要求男生按年龄升序排序,女生按年龄降序排序
	* 多表查询,多张表的结构是完全一样的,保持的数据结构也是一样的
* 排序
	* 联合查询中使用order by,我们必须将select语句用括号括起来
	```js
	-- 在 student 表中,按年龄,男升女降
	(select * from student where gender = "boy" order by age asc)
		union
	(select * from student where gender = "girl" order by age desc);
	```


### 子查询 sub query
查询是在某个查询结果之上进行的,一条select语句内部包含了另外一条select语句

### 视图 view
是一种有结构(有行有列),但没有结果(结构中不真实存放数据)的虚拟表,虚拟表的结构来源不是自己定义的,而是从对应的基表(视图的数据来源)中产生的
* `create view + 视图名 + as + select语句;`










### 主键冲突
---
在数据插入的时候假设主键对应的值已经存在,则插入失败!这就是主键冲突
---
> 解决方法
> 更新或替换

* `insert into + 表名 + [(字段列表：包含主键)] + values (值列表) on duplicate key update 字段 = 新值;`
* `insert into my_class values ('PM3527','B315') on duplicate key update`
* 类似一个update更新, 不改变主键, 只更新提交更新的字段
* `replace insert into + 表名 + [(字段列表：包含主键)] + values (值列表);`
* `replace into my_class values ('PM3528','B215')`

### 蠕虫复制
---
从已有的数据表中获取数据,然后将数据进行新增操作,数据成倍(以指数形式)的增加
---
* `create table + 表名 + like + [数据库名.]表名;`
* -- 根据已有表,创建新表,当两张表位于同一数据库时,可以省略数据库名称
* `create table my_copy like my_gbk;`
* 表`my_copy`和表`my_gbk`的表结构完成相同


* `insert into + 表名 + [()] + select + 字段列表/* + from + 表名;`
* `insert into my_copy select * from my_collate_bin;`


## 数据备份与还原
防止数据丢失; 保护数据记录

* 备份 将当前已有的数据或记录另存一份
* 还原将数据恢复到备份时的状态

* 数据表备份
* 单表数据备份
* SQL备份和增量备份

存储引擎: MySQL 主要使用两种: 默认InnoDB , Myisam


## 事务

> 案例：银行的数据库里面存储着用户的账户信息表,当用户 A 想用户 B 转账的时候,正常情况下,A 账户的余额减少,B 账户的余额增加；但是由于某种原因(例如突然断电),当 A 账户的余额减少之后,B 账户的余额并没有增加,这就造成了数据库数据的安全隐患.
> 解决方案：当 A 账户的余额减少之后,不要立即修改数据表,而是在确认 B 账户的余额增加之后,同时修改数据表
一系列将要发生或正在发生的连续操作
* 事务安全,是一种保护连续操作同时实现(完成)的机制
* 事务安全的意义:保证数据操作的完整性

```js
1. start transaction;  开启事务
2.update bank_account set money = money - 1000 where id = 1; // -- 更新 Charies 账户余额
3.update bank_account set money = money + 1000 where id = 2; 更新 Gavin 账户余额
4. commit; 提交事务
4. rollback; 或回滚事务

如果我们选择提交事务,则将事务日志存储的记录直接更新到数据库,并清除事务日志；
如果我们选择回滚事务,则直接将事务日志清除,所有在开启事务至回滚事务之间的操作失效,保持原有的数据库记录不变
```
>当我们提交事务之后,在进行回滚事务是不起作用的,因为事务日志在提交事务的同时已经被清除啦！
>现阶段,只有 InnoDB 和 BDB 两个存储引擎是支持事务安全机制的,其中 InnoDB 免费,BDB 收费.因此,InnoDB 使用的最为广泛

> 设置回滚点 savepoint + 回滚点名称
> 返回回滚点 rollback to + 回滚点名称


## 触发器 trigger
> 网上购物，根据生产订单的类型，商品的库存量对应的进行增和减。此案例涉及两张表，分别为订单表和商品表，下单时，商品表库存减少；退单时，商品表库存增加

事先为某张表绑定一段代码，当表中的某些内容发生改变（增、删、改）的时候，系统会自动触发代码并执行。
* 事件类型：增删改，即insert、delete和update
* 触发时间：事件类型前和后，即before和after；
* 触发对象：表中的每一条记录（行），即整张表

每张表只能拥有一种触发时间的一种事件类型的触发器，即每张表最多可以拥有 6 种触发器

## 代码执行结构
* 顺序结构
* 分支结构
* 循环结构

## 函数
>就是将一段代码封装到一个结构中，在需要执行该段代码的时候，直接调用该结构（函数）执行即可。此操作，实现了代码的复用。在 MySQL 中，函数有两种，分别为

* 系统函数
* 自定义函数

## 存储过程 procedure


