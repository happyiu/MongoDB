# MongoDB 基础

## MongoDB 概念

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

## MongoDB 连接
- 在命令行工具下: > mongo  就能进入数据库
- 连接本地数据库服务器，端口默认
  - > mongodb://localhost
- 使用用户名密码登录指定的数据库
  - > mongoose.connect('mongodb://i_blog_owner:1234@127.0.0.1:29999/blog',{useMongoClient:true}) 


## 数据库操作

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

### 






