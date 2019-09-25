# MongoDB
```js
{
    // 数据库
    qq:{
        // 集合数组(数据表)
        user:[
            // 数据
            {name:'jack',age:12},
            {name:'jack2',age:13},
            {name:'jack3',age:16},
        ],
        product:[

        ]
        
    },
    tabao{

    }
}
```

## 关系型数据库和非关系型数据库

### 关系型数据库
- 表就是关系，所有关系型数据库都需要通过 sql 语言来操作
- 所有的关系型数据库都需要设计表结构，数据表还支持约束

### 非关系型数据库
- 非关系型数据库非常灵活
- 有的非关系数据库就是 key-value 对
- 没有表的概念
- 在 MongoDB 是长得的最像关系型数据库的非关系型数据库
    - 数据库 -> 数据库
    - 数据表 -> 集合（数组）
    - 表记录 -> 文档对象
- MongoDB 不需要设计表结构
- 可以任意的往里面存数据，没有结构性这么一说

## MongoDB下载安装
- 下载好后，需要将目录下的bin的路径添加到环境变量中
- 最后cmd输入：mongod --version

# MongoDB 使用
- 启动和关闭数据库(控制台输入)
    - 启动：mongod
    - 关闭：ctrl+c
- MongoDB 默认使用 mongod 命令所在磁盘根目录下的 /data/db 作为自己的数据存储目录
- 所以第一次需要自己手动新建一个 /data/db
- 修改默认的数据存储目录
    > mongod --depath=数据存储目录

# MongoDB 连接
- 连接MongoDB(控制台输入)
    > mongo
- 在连接时，退出连接
    > exit

# MongoDB 命令提示符的基本命令

## show dbs
查看显示所有数据库

## db
查看当前操作的数据库

## use 数据库名称
切换到指定的数据库，如果没有会新建数据库（即先进入这个名字的数据库，但没建立,有数据进入后，才会新建出来）

## 插入数据
db.students.insertOne({"name":"jack"})

## show collections
查看当前数据库所有集合

## db.集合名.find()
查询该数据库该集合名下所有的数据

# node 中操作 MongoDB 

## 使用官方的 MongoDB包 来操作（原生）
## 使用 第三方 mongoose 来操作 MongoDB 数据库
第三方包基于原生MongoDB包封装

## 使用步骤
```js
// 1. npm安装mongoose，引入包
var mongoose = require('mongoose')
// Mongoose 里，一切都始于Schema
var Schema = mongoose.Schema

// 2. 指定连接的数据库不需要存在，当你插入第一条数据后就会创建出来
mongoose.connect('mongodb://localhost/itcast')

// 3. 设计集合结构(表结构)
// 约束的目标是保证数据的完整性
var userSchema = new Schema({
    username:{
        type:String,
        required:true
    },
    password:{
        type:String,
        required:true
    },
    email:{
        type:String
    }
})

// 4. 将集合结构发布为模型
// mongoose.model() 方法就是将一个架构发布为model
// 第一个参数：传入一个大写名词单数字符串来表示你数据库的名称，mongoose自动将大写名词生成为小写复数的集合名称
// 第二个参数：架构Schame
// 返回一个模型架构函数
var User = mongoose.model('User',userSchema)

```

- 保存数据
```js
// 增加
var admin = new User({
    username:"admin",
    password:"123456",
    email:"admin123456@qq.com"
})
admin.save((err,ret)=>{
    if(err){
        console.log("保存失败")
    } else{
        console.log("保存成功")
        console.log(ret)
    }
})
```

- 查询数据
```js
// 查询所有数据，得到数组，即使查不到，会返回一个空数组
User.find((err,ret)=>{
    if(err){
        console.log("查询失败")
    } else{
        console.log(ret)
    }
})
// 按条件查询，得到数组
User.find({
    username:'张三'
},(err,ret)=>{
    if(err){
        console.log("查询失败")
    } else{
        console.log(ret)
    }
})
// 按条件只找一个,得到对象
User.findOne({
    username:'张三',
    password:'1236'
},(err,ret)=>{
    if(err){
        console.log("查询失败")
    } else{
        console.log(ret)
    }
})
```

- 删除
```js
// 按条件删除数据
User.remove({
    username:'张三'
},(err,ret)=>{
    if(err){
        console.log("删除失败")
    } else{
        console.log("删除成功")
    }
})
```

- 更新数据
```js
// 根据id更新数据
User.findByIdAndUpdate('5d4152f22abf570a0089e386',{
    password:'123'
},(err,ret)=>{
    if(err){
        console.log('更新失败')
    } else{
        console.log('更新成功')
    }
})
```