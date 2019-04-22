## sequelize
[sequelize文档](http://docs.sequelizejs.com)
### 安装

```sh
npm install --save sequelize
# And one of the following:
npm install --save pg pg-hstore
npm install --save mysql2
npm install --save sqlite3
npm install --save tedious // MSSQL

```
### 连接

```js
const Sequelize = require('sequelize');
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
	port: port,
  dialect: 'mysql'|'sqlite'|'postgres'|'mssql',
  operatorsAliases: false,

  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  },

  // SQLite only
  storage: 'path/to/database.sqlite'
});
sequelize
  .authenticate()
  .then(() => {
    console.log('Connection has been established successfully.');
  })
  .catch(err => {
    console.error('Unable to connect to the database:', err);
  });
```
### model

```js
const User = sequelize.define('user', {
  firstName: {
    type: Sequelize.STRING
  },
  lastName: {
    type: Sequelize.STRING
  }
}, {
  freezeTableName: true // 模型名字与表名相同
});

// 创建表
// force: true 如果表已经存在，将会丢弃表
User.sync();
// User.sync({force: true})
```
#### mdoel 类型
以下是 Sequelize 支持的一些数据类型.有关完整和更新的列表, 参阅[DataTypes](http://docs.sequelizejs.com/variable/index.html#static-variable-DataTypes)
```js
Sequelize.STRING                      // VARCHAR(255)
Sequelize.STRING(1234)                // VARCHAR(1234)
Sequelize.STRING.BINARY               // VARCHAR BINARY
Sequelize.TEXT                        // TEXT
Sequelize.TEXT('tiny')                // TINYTEXT

Sequelize.INTEGER                     // INTEGER
Sequelize.BIGINT                      // BIGINT
Sequelize.BIGINT(11)                  // BIGINT(11)

Sequelize.FLOAT                       // FLOAT
Sequelize.FLOAT(11)                   // FLOAT(11)
Sequelize.FLOAT(11, 12)               // FLOAT(11,12)

Sequelize.REAL                        // REAL         仅限于PostgreSQL.
Sequelize.REAL(11)                    // REAL(11)     仅限于PostgreSQL.
Sequelize.REAL(11, 12)                // REAL(11,12)  仅限于PostgreSQL.

Sequelize.DOUBLE                      // DOUBLE
Sequelize.DOUBLE(11)                  // DOUBLE(11)
Sequelize.DOUBLE(11, 12)              // DOUBLE(11,12)

Sequelize.DECIMAL                     // DECIMAL
Sequelize.DECIMAL(10, 2)              // DECIMAL(10,2)

Sequelize.DATE                        // DATETIME 针对 mysql / sqlite, TIMESTAMP WITH TIME ZONE 针对 postgres
Sequelize.DATE(6)                     // DATETIME(6) 针对 mysql 5.6.4+. 小数秒支持多达6位精度
Sequelize.DATEONLY                    // DATE 不带时间.
Sequelize.BOOLEAN                     // TINYINT(1)

Sequelize.ENUM('value 1', 'value 2')  // 一个允许具有 “value 1” 和 “value 2” 的 ENUM
Sequelize.ARRAY(Sequelize.TEXT)       // 定义一个数组。 仅限于 PostgreSQL。
Sequelize.ARRAY(Sequelize.ENUM)       // 定义一个 ENUM 数组. 仅限于 PostgreSQL。

Sequelize.JSON                        // JSON 列. 仅限于 PostgreSQL, SQLite and MySQL.
Sequelize.JSONB                       // JSONB 列. 仅限于 PostgreSQL .

Sequelize.BLOB                        // BLOB (PostgreSQL 二进制)
Sequelize.BLOB('tiny')                // TINYBLOB (PostgreSQL 二进制. 其他参数是 medium 和 long)

Sequelize.UUID                        // PostgreSQL 和 SQLite 的 UUID 数据类型, CHAR(36) BINARY 针对于 MySQL (使用默认值: Sequelize.UUIDV1 或 Sequelize.UUIDV4 来让 sequelize 自动生成 ID)

Sequelize.CIDR                        // PostgreSQL 的 CIDR 数据类型
Sequelize.INET                        // PostgreSQL 的 INET 数据类型
Sequelize.MACADDR                     // PostgreSQL 的 MACADDR

Sequelize.RANGE(Sequelize.INTEGER)    // 定义 int4range 范围. 仅限于 PostgreSQL.
Sequelize.RANGE(Sequelize.BIGINT)     // 定义 int8range 范围. 仅限于 PostgreSQL.
Sequelize.RANGE(Sequelize.DATE)       // 定义 tstzrange 范围. 仅限于 PostgreSQL.
Sequelize.RANGE(Sequelize.DATEONLY)   // 定义 daterange 范围. 仅限于 PostgreSQL.
Sequelize.RANGE(Sequelize.DECIMAL)    // 定义 numrange 范围. 仅限于 PostgreSQL.

Sequelize.ARRAY(Sequelize.RANGE(Sequelize.DATE)) // 定义 tstzrange 范围的数组. 仅限于 PostgreSQL.

Sequelize.GEOMETRY                    // 空间列.  仅限于 PostgreSQL (具有 PostGIS) 或 MySQL.
Sequelize.GEOMETRY('POINT')           // 具有几何类型的空间列.  仅限于 PostgreSQL (具有 PostGIS) 或 MySQL.
Sequelize.GEOMETRY('POINT', 4326)     // 具有几何类型和SRID的空间列.  仅限于 PostgreSQL (具有 PostGIS) 或 MySQL.
```
#### mdoel defaultValue


#### find 使用
##### find 搜索数据库中的一个特定元素
```js
// 搜索已知的ids
Project.findById(123).then(project => {
  // project 将是 Project的一个实例，并具有在表中存为 id 123 条目的内容。
  // 如果没有定义这样的条目，你将获得null
})

// 搜索属性
Project.findOne({ 
		where: {
			title: 'aProject'
		} 
	}).then(project => {
  // project 将是 Projects 表中 title 为 'aProject'  的第一个条目 || null
})


Project.findOne({
  where: {title: 'aProject'},
  attributes: ['id', ['name', 'title']]
}).then(project => {
  // project 将是 Projects 表中 title 为 'aProject'  的第一个条目 || null
  // project.title 将包含 project 的 name
})
```
##### findOrCreate 搜索特定元素或创建它（如果不可用）
我们有一个空的数据库，一个 User 模型有一个 username 和 job
```js
User
  .findOrCreate({
		where: {username: 'sdepold'}, 
		defaults: {job: 'Technical Lead JavaScript'}
	})
  .spread((user, created) => {
    console.log(user.get({
      plain: true
    }))
    console.log(created)
    /*
    findOrCreate 返回一个包含已找到或创建的对象的数组，找到或创建的对象和一个布尔值，如果创建一个新对象将为true，否则为false，像这样:

    [ {
        username: 'sdepold',
        job: 'Technical Lead JavaScript',
        id: 1
      },
      true ]

在上面的例子中，".spread" 将数组分成2部分，并将它们作为参数传递给回调函数，在这种情况下将它们视为 "user" 和 "created" 。（所以“user”将是返回数组的索引0的对象，并且 "created" 将等于 "true"。）
    */
  })

```
创建了一个新的实例.所以当我们已经有一个实例了
```js
User.create({ username: 'fnord', job: 'omnomnom' })
  .then(() => User.findOrCreate({
		where: {username: 'fnord'}, 
		defaults: {job: 'something else'}
	}))
  .spread((user, created) => {
    console.log(user.get({
      plain: true
    }))
    console.log(created)

    /*
    在这个例子中，findOrCreate 返回一个如下的数组：
    [ {
        username: 'fnord',
        job: 'omnomnom',
        id: 2
      },
      false
    ]
    由findOrCreate返回的数组通过 ".spread" 扩展为两部分，并且这些部分将作为2个参数传递给回调函数，在这种情况下将其视为 "user" 和 "created".（所以“user”将是返回数组的索引0的对象，并且 "created" 将等于 "false"。）
    */
  })
```
##### findAndCountAll - 在数据库中搜索多个元素，返回数据和总计数
处理程序成功将始终接收具有两个属性的对象：
* count - 一个整数，总数记录匹配where语句和关联的其它过滤器
* rows - 一个数组对象，记录在limit和offset范围内匹配where语句和关联的其它过滤器

```js
Project
  .findAndCountAll({
		where: {
			title: {
				[Op.like]: 'foo%'
			}
		},
		offset: 10,
		limit: 2
  })
  .then(result => {
		console.log(result.count)
		console.log(result.rows);
  })

```
它支持 include。 只有标记为 required 的 include 将被添加到计数部分：

假设您想查找附有个人资料的所有用户：
```js
User.findAndCountAll({
  include: [
     { model: Profile, required: true}
  ],
  limit: 3
});
```
因为 Profile 的 include 有 required 设置，这将导致内部连接，并且只有具有 profile 的用户将被计数。 如果我们从 include 中删除required，那么有和没有 profile 的用户都将被计数。 在include中添加一个 where 语句会自动使它成为 required：
```js
User.findAndCountAll({
  include: [
     { model: Profile, where: { active: true }}
  ],
  limit: 3
});
```
上面的查询只会对具有 active profile 的用户进行计数，因为在将 where 语句添加到 include 时，required 被隐式设置为 true

传递给 findAndCountAll 的 options 对象与 findAll 相同（如下所述）
##### findAll - 搜索数据库中的多个元素
```js
// 找到多个条目
Project.findAll().then(projects => {
  // projects 将是所有 Project 实例的数组
})

// 也可以：
Project.all().then(projects => {
  // projects 将是所有 Project 实例的数组
})

// 搜索特定属性 - 使用哈希
Project.findAll({ where: { name: 'A Project' } }).then(projects => {
  // projects将是一个具有指定 name 的 Project 实例数组
})

// 在特定范围内进行搜索
Project.findAll({ where: { id: [1,2,3] } }).then(projects => {
  // projects将是一系列具有 id 1,2 或 3 的项目
  // 这实际上是在做一个 IN 查询
})

Project.findAll({
  where: {
    id: {
      [Op.and]: {a: 5},           // 且 (a = 5)
      [Op.or]: [{a: 5}, {a: 6}],  // (a = 5 或 a = 6)
      [Op.gt]: 6,                // id > 6
      [Op.gte]: 6,               // id >= 6
      [Op.lt]: 10,               // id < 10
      [Op.lte]: 10,              // id <= 10
      [Op.ne]: 20,               // id != 20
      [Op.between]: [6, 10],     // 在 6 和 10 之间
      [Op.notBetween]: [11, 15], // 不在 11 和 15 之间
      [Op.in]: [1, 2],           // 在 [1, 2] 之中
      [Op.notIn]: [1, 2],        // 不在 [1, 2] 之中
      [Op.like]: '%hat',         // 包含 '%hat'
      [Op.notLike]: '%hat',       // 不包含 '%hat'
      [Op.iLike]: '%hat',         // 包含 '%hat' (不区分大小写)  (仅限 PG)
      [Op.notILike]: '%hat',      // 不包含 '%hat'  (仅限 PG)
      [Op.overlap]: [1, 2],       // && [1, 2] (PG数组重叠运算符)
      [Op.contains]: [1, 2],      // @> [1, 2] (PG数组包含运算符)
      [Op.contained]: [1, 2],     // <@ [1, 2] (PG数组包含于运算符)
      [Op.any]: [2,3],            // 任何数组[2, 3]::INTEGER (仅限 PG)
    },
    status: {
      [Op.not]: false,           // status 不为 FALSE
    }
  }
})
```
```js
// 限制字段
var users = await User.findAll({
	'attributes': ['emp_id', 'nick']
});
SELECT `emp_id`, `nick` FROM `users`;

// 字段重命名
var users = await User.findAll({
	'attributes': [
		'emp_id', ['nick', 'user_nick']
	]
})
SELECT `emp_id`, `nick` AS `user_nick` FROM `users`;

// where子句
var users = await User.findAll({
	'where': {
		'id': [1, 2, 3],
		'nick': 'a',
		'department': null
	}
})
SELECT `id`, `emp_id`, `nick`, `department`, `created_at`, `updated_at` 
FROM `user` 
WHERE 
    `user`.`id` IN (1, 2, 3) AND 
    `user`.`nick`='a' AND 
    `user`.`department` IS NULL;
```
---
操作符 where
```js
'$eq': 1,                //  = 1
'$ne': 2,                //  != 2
'$gt': 6,                //  > 6
'$gte': 6,               //  >= 6
'$lt': 10,               //  < 10
'$lte': 10,              //  <= 10
'$between': [6, 10],     //  BETWEEN 6 AND 10
'$notBetween': [11, 15], //  NOT BETWEEN 11 AND 15
'$in': [1, 2],           //  IN (1, 2)
'$notIn': [3, 4]         //  NOT IN (3, 4)
'$like': '%a%',          //  LIKE '%a%'
'$notLike': '%a'         //  NOT LIKE '%a'
'$eq': null,             //  IS NULL
'$ne': null              //  IS NOT NULL
```
---
* $and
* $or
* $not
* order 排序
* 分页 User.findAll({
	'limit': countPerPage, // 每页多少条 
	'offset': countPerPage * (currentPage - 1)// 跳过多少条 
})




```js
// OR条件
var users = await User.findAll({
    'where': {
        '$or': [
            {'id': [1, 2]},
            {'nick': null}
        ]
    }
})
SELECT `id`, `emp_id`, `nick`, `department`, `created_at`, `updated_at` 
FROM `user` WHERE 
(
    `user`.`id` IN (1, 2) OR 
    `user`.`nick` IS NULL
)
```
```js
// AND条件
var users = await User.findAll({
    'where': {
        '$and': [
            {'id': [1, 2]},
            {'nick': null}
        ]
    }
})
SELECT `id`, `emp_id`, `nick`, `department`, `created_at`, `updated_at` 
FROM `user` WHERE 
(
    `user`.`id` IN (1, 2) AND 
    `user`.`nick` IS NULL
)
```
```js
// NOT条件
var users = await User.findAll({
    'where': {
        '$not': [
            {'id': [1, 2]},
            {'nick': null}
        ]
    }
})
```
```js

```
```js

```
```js

```
```js

```


#### 改 update 使用
```js
// 方法1：操作对象属性（不会操作db），调用save后操作db
user.nick = '小白';
user = yield user.save();
console.log(user.get({'plain': true}));
 
// 方法2：直接update操作db
user.update({
    'nick': '小白白'
}).then(user => {
	console.log(user.get({'plain': true}))
})
```

#### ffff

```js
User.findAll().then((users) => {
	console.log(users)
})
findAll
findOne
findById
findOrCreate
findAndCountAll

findAll({
	where: {
		title: 'afacode'
	},
	attributes: ['id', ['name', title]]
})

findOrCreate({
	where: {
		username: 'afacode'
	},
	defaults: {
		job: 'FE'
	}
}).spread(user, created) => {
	
}

```
* new Sequelize
* sequelize.models 返回通过sequelize.define定义的所有模型对象
* sequelize.define
* sequelize.Utils
* sequelize.Promise
* sequelize.QueryTypes 查询类型枚举
* sequelize.Validator
* sequelize.Transaction 事务对象

project.max('age')

project.min('age')

project.sum('age')
















