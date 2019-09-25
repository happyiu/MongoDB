# MongoDB 基础

## 目录

[MongoDB 概念](#gn)

[MongoDB 连接](#connect)

[MongoDB 操作](#cz)


## <a id="gn">MongoDB 概念</a> 

### MongoDB 与 SQL 区别

|  SQL概念  |  MongoDB概念   |       说明     |
|-----------|---------------|----------------|
| database  |  database     |  数据库        |
| table     |  collection   |  数据库表/集合  |
| row       |  document     |  数据记录行/[文档](#document)  |
| column    |  field        |  数据字段/域    |
| index     |  index        |  索引          |
|primary key|  primary key  |  主键，MongoDB自动将_id设置为主键  |
|   表联合   |   嵌入文档    |  主键，MongoDB自动将_id设置为主键  |


选择指定的数据库

### <a id="document">文档</a>
文档就是一组键值对：{"id":123,"name":"李四"}

## <a id="connect">MongoDB 连接</a>
- 在命令行工具下: > mongo  就能进入数据库
- 连接本地数据库服务器，端口默认
  - > mongodb://localhost
- 使用用户名密码登录指定的数据库
  - > mongoose.connect('mongodb://i_blog_owner:1234@127.0.0.1:29999/blog',{useMongoClient:true}) 


## <a id="cz">数据库操作</a>

### use 数据库名
use 数据库名 创建数据库 或 切换数据库

使用use关键字，若数据库不存在则创建数据库，若存在就切换到数据库

### show dbs
显示所有数据库

### db
显示当前数据库对象

### db.dropDatabase()
删除数据库（前提是 已经使用 use 进入该数据库）

### db.createCollection(name,options)
创建集合（前提是 已经使用 use 进入该数据库）(如果不创建集合就插入文档，那么会自动创建集合)
name：集合名
options：可选，指定有关内存大小及索引的选项
    - capped(boolean)  创建固定集合（有固定大小的集合）
    - autoIndex(boolean)  true 自动在 _id字段创建索引，默认为false
    - size  为固定集合指定最大值
    - max  指定固定集合包含文档的最大数量

```
use school
db.createColletion("students",{capped:true,autoIndex:true,size:6142800,max:10000})
```

### show collections
查看该数据库下的所有集合（前提是 已经使用 use 进入该数据库）

### db.集合名.drop()
删除集合

### db.集合名.insert() 
插入文档
```
db.school.insert([{name:'小明',age:18,sex:1},{name:'小明2',age:18,sex:1}])
```
也可以将数据定义为一个变量，再执行操作
```
document = ({
    name:'小红',
    age:15,
    sex:0
})

db.school.inster(document)
```
#### db.集合名.insetOne() 
插入一条文档数据

#### db.集合名.insetMany() 
插入多条文档数据

### db.集合名.find()
查看已插入的文档
```
db.school.find()
```

### db.集合名.update()
更新已存在的文档

db.students.update(
    <query>,        // 查询条件
    <update>,       // 更新的对象和更新的操作符
    {
        upsert:<boolean>,   // 如果不存在update的记录，是否插入数据
        multi:<boolean>,    // 默认flase，只查找符合的第一条
        writeConcern:<document>     // 抛出异常的级别
    }
)

```
db.students.update({"name":"小明"},{$set:{"name":"小明2"}})
```

### db.集合名.save()
更新已存在的文档

db.studnets.save(
    <document>
)

```
db.students.save(
    {
        "_id":ObjectId("5d89eb537546512aa0cd063e"),
        "uId":1622120069,
        "name":"小明2",
        "age":11,
        "sex":1,
        "cId":2
    }
)
```

### db.集合名.remove()
删除文档
db.集合名.remove(
    <query>,    删除文档的条件
    {
        justOne:<boolean>,  默认false，true 则只匹配一项
        writeConcern:<document>     抛出异常的级别
    }
)

```
db.studnet.remove(
    {"name":"hy"}
)
```

### db.集合名.find()
db.students.find({},{"age":11,"name":''})   只显示显示age<10的数据，且返回的查询文档只有默认的_id，符合条件的age，和对应的name
db.student.find().pretty()  以格式化的方式显示所有文档

```
db.students.find({"sex":{$gt:0}})  查找sex值大于0的文档
```

### 查询条件
- 等于：db.student.find({"age":18})     age = 18
- 小于：db.student.find({"age":{$lt:10}})     age < 10
- 小于等于：db.student.find({"age":{$lte:10}})     age <= 10
- 大于：db.student.find({"age":{$gt:10}})     age > 10
- 大于等于：db.student.find({"age":{$gte:10}})     age >= 10
- 不等于：db.student.find({"age":{$ne:10}})     age != 10
- and：db.student.find({"name":"hy","age":18})  name = hy && age = 18
- or：db.student.find({$or:[{"name":"hy"},{"age":18}]})     name = hy || age = 18
- 范围：db.student.find({"age":{$gt:10,$lt:18}})    10 < a < 18
- $type实例：db.student.find({"age":{$type:'string'}})    age值类型为string的文档
- 查询指定的数据条数：db.studnet.find().limit(2)       查询2条数据，不传值则默认查询全部数据
- 跳过指定的查询条数：db.student.find().skip(2)     查询前2条后的数据

### db.student.find().sort({key:1/-1})
排序  1为升序，-1位降序
db.student.find().sort({"age":1})  按age值的大小进行升序排序

### 索引 
#### db.集合名.createIndex(key,options)
创建集合    1为升序，-1为降序
db.student.createIndex({"age":1,"sex":1})  多个字段创建索引

#### db.集合名.getIndexes()
查看索引

#### db.集合名.totalIndexSize()
查看集合索引的大小

#### db.集合名.dropIndexes()
删除集合的所有索引

#### db.集合名.dropIndex()
删除集合指定的索引

### 聚合
主要用来处理数据（求平均值，求和）并返回结果
db.student.aggregate()
- $sum 计算总和
- $avfg 计算平均值
- $min 获取最小值
- $max 获取最大值
- $push 将结果文档中插入值到一个数组
- $addToSet 将结果文档中插入值到一个数组，但不创建副本
- $first 根据资源文档的排序获取第一个文档数据
- $last 根据资源文档的排序获取最后一个文档数据

```
                        分组    主键       计算的结果
db.student.aggregate([{$group:{_id:"sex",num:{$sum:1}}}])  将sex分组并统计每组的个数
```

### 多表关联
db.student.aggregate([
    {
        $lookup:
        {
            from:"同一个数据库下待被Join的集合",
            localField: "源集合中要匹配的属性",
            foreignField: "待Join的集合中要匹配的属性",
            as: "输出文档的命名"
        }
    }
])
 
```
db.students.aggregate([
    {
        $lookup:
        {
            form:"class",
            localField:"cid",
            foreignField:"cid",
            as:"stucalss"
        }
    }
])
```


