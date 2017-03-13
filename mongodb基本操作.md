### 创建数据库

`use DATABASE_NAME` 如果数据库不存在则创建，否则切换到指定数据库

`db`查看当前数据库名

`show dbs` 查看所有数据库

`show tables` 查看集合

### 删除数据库

`db.dropDatabase()` 删除当前数据库

`db.collection.drop()`  删除集合，collection为集合名，例`db.student.drop()`

### 插入文档
`db.COLLECTION_NAME.insert(document)`
实例
```javascript
db.col.insert({
	title: 'MongoDB教程',
	tags: ['mongodb', 'database', 'nosql'],
	url: 'www.runoob.com'
})
```


我们也可以把数据定义为变量

```javascript
document = ({  // 记得这边有个括号
	title: 'MongoDB教程',
	tags: ['mongodb', 'database', 'nosql'],
	url: 'www.runoob.com'
})

db.col.insert(document)
```

插入文档也可以使用	`db.col.save(document)` 命令，如果不指定 `_id` 字段save()方法类似
于 insert()。如果指定 `_id` 字段，则会更新该 `_id` 的数据。

### 更新文档

#### update()方法

```jvascript
db.collection.update(
	<query>,  // 查询条件
	<update>,  //update的对象和一些更新的操作符
	{
		upsert: <boolean>,  // 可选
		multi: <boolean>,   // 可选
		writeConcern: <document>  // 可选，抛出异常的级别
	}
)
```

例子
`db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})`

以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置multi参数为true

#### save()方法

```
db.collection.save(
	<document>, // 传入文档用来替换之前的文档
	{
		writeConcern: <document>
	}
)
```

例子
```
db.col.save({
	"_id" : ObjectId("56064f89ade2f21f36b03136"),  // id唯一
    "title" : "MongoDB",
    "description" : "MongoDB 是一个 Nosql 数据库",
    "by" : "Runoob",
    "url" : "http://www.runoob.com",
    "tags" : [
            "mongodb",
            "NoSQL"
    ],
    "likes" : 110
})
```

### 删除文档

```
db.collection.remove(
	<query>,       // 可选，删除的文档的条件
	{
		justOne:<boolean>,   // 可选，如果为true或1，则只删除一个文档
		writeConcern: <document>  // 可选，抛出异常级别
	}
)
```

例子
`db.col.remove({'title':'mongodb'})`

`db.col.remove({})` 删除所有数据

### 查询文档
`db.col.find()` 查看已插入的文档
`db.col.find().pretty()`  输出好看的格式
`db.col.findOne()` 只返回一个文档

### 条件操作符
(>) 大于 - $gt
(<) 小于 - $lt
(>=) 大于等于 - $gte
(<= ) 小于等于 - $lte
`db.col.find({"likes" : {$gt : 100}})` 查找likes大于100的数据

#### AND条件
传入多个键值然后逗号隔开
`db.col.find({key1: value1, key2: value2}).pretty()`

#### OR条件
```db.col.find({$or
	[
		{key1: value1}, {key2: value2}
	]
}).pretty()
```

### $type操作符
| 类型 | 数字 |
| --- | --- |
| Double | 1 |
| String | 2 |
| Object | 3 |
| Array | 4 |
| Binary data | 5 |
| Object id | 7 |
| Boolean | 8 |
| Date | 9 |
| Null | 10 |

不全，[完整表格访问这里](http://www.runoob.com/mongodb/mongodb-operators-type.html)

`db.col.find({'title' :{$type: 2}})` 如果title为String则输出

### limit and skip
`db.col.find().limit(2)`  只读取两条
`db.col.find().limit(1).skip(1)` 跳过第一条，只显示第二条
skip默认为0

### sort()方法

1升序，-1降序

`db.col.find().sort({key: 1})`

例子
`db.col.find({}, {'title': 1, _id: 0}).sort({'likes': -1})`
