# sequelize 基本操作

[Sequelize](http://docs.sequelizejs.com/) 是 Node 的一个 ORM(Object-Relational Mapping) 框架，用来方便数据库操作。

## 配置 sequelize

>以 mysql 为例

首先我们要引入npm包，sequelize 依赖 mysql2 作为底层驱动，暴露出自己的 API 让我们调用，在转成 mysql 语句进行执行。

```json
"mysql2": "^1.5.1",
"sequelize": "^4.28.6"
```

```javascript
const Sequelize = require('sequelize')

// 连接数据库
const sequelize = new Sequelize('database', 'username', 'password', {
  host: sqlconf.host,
  dialect: 'mysql', // 这里可以改成任意一种关系型数据库

  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000,
  },
})

// 测试连接是否成功
sequelize
  .authenticate()
  .then(() => {
    console.log('Connection has been established successfully.')
  })
  .catch(err => {
    console.log('Unable to connect to the database', err)
  })

// 根据 model自动创建表
sequelize
  .sync()
  .then(() => {
    console.log('init db ok')
  })
  .catch(err => {
    console.log('init db error', err)
  })
```

我们可以调用`sync()`根据 model自动在数据库中创建表，也可以不调用，自己手动创。如果使用了 Sequelize 的 [Associations](http://docs.sequelizejs.com/manual/tutorial/associations.html)，这必须通过 `sync()` 生成表结构。

## 创建 model

创建模型，告诉 Sequelize 如何映射数据库表

```javascript
const UserModel = sequelize.define('user', {
  id: {
    type: Sequelize.INTEGER(11),
    primaryKey: true,            // 主键
    autoIncrement: true,         // 自动递增
  },
  username: Sequelize.STRING(100),
  password: Sequelize.STRING(100),
  createdAt: Sequelize.BIGINT,
  updatedAt: Sequelize.BIGINT,
}, {
  timestamps: false
})
```

`define()` 方法的第一个参数为表名，对应的是 **users** 表。如果不设置 timestamps，Sequlize 会自动为我们添加创建时间和更新时间，我一般选择关闭，手动添加灵活性高些。

## 增删改查

### 增

```javascript
(async () => {
  const now = Date.now()
  const user = await UserModel.create({
    username: '小张',
    password: 'root',
    createAt: now,
    updateAt: now,
  })
  console.log('创建：' + JSON.stringify(user))
})();
```

### 查

```javascript
(async () => {
  // 查找所有
  const allUser = await UserModel.findAll()

  // 按id查找
  const oneUser = await UserModel.findById(id)

  // 按条件查询
  const someUser = await UserModel.findAll({
    where: {
      // 模糊查询
      name: {
        $like: '%小%',
      },

      // 精确查询
      password: 'root',
    }
  })

  // 分页查询
  const size = 10 // 每页10条数据
  const page = 1 // 页数
  const pageUser = await UserModel.findAndCountAll({
    where: {
      name: {
        $like: '%小%',
      },
    },
    limit: size,
    offset: size * (page - 1),
  })
})();
```

### 改

```javascript
(async () => {
// 方法一
await UserModel.upert(data)  // data 里面如果带有 id 则更新，不带则新建

// 方法二
const user = await UserModel.findById(id)
user.update(data)
})()
```

### 删

```javascript
(async () => {
// 方法一
// 删除所有名字带’小‘的用户
await UserModel.destroy({
  where: {
    username: '小',
  },
})

// 方法二
const user = await UserModel.findById(id)
user.destroy()
})()
```

## 关联表

Sequelize 提供了一对一，一对多，多对多等关联表操作，我用的不多，这里只介绍 `hasMany()` 这一种，其他的可以看[文档](http://docs.sequelizejs.com/manual/tutorial/associations.html)。

### 设置
首先要在 model 中设置

```javascript
const School = sequelize.define('school', {
  id: {
    type: Sequelize.INTEGER(11),
    primaryKey: true,
    autoIncrement: true,
  },
  username: Sequelize.STRING(100),
})

const Student = sequelize.define('student', {
    id: {
    type: Sequelize.INTEGER(11),
    primaryKey: true,
    autoIncrement: true,
  },
  username: Sequelize.STRING(100),
})

School.hasMany(Student, {as: 'student', foreignKey: 'schoolId'})
```

as 参数重新定义了目标model的名字。foreignKey 参数定义了在 t_student 表中关联 key 的名字。

### 关联查

如果我们想查找一个学校和这个学校中所有的学生信息，可以这样找：

```javascript
(async () => {
  const group = await School.findById(id, {
    include: [{
      model: Student,
      as: 'student',
    }],
  })
})()
```

如果我们设置了 as 就需要在 include 选项中设置同样的 as。

按条件查找

```javascript
(async () => {
  const group = await School.findAll({
    where: {
      name: 'someting',
    },
    include: [{
      model: Student,
      as: 'student',
    }],
  })
})()
```

## 文档

更多详细操作请参考[官方文档](http://docs.sequelizejs.com/)