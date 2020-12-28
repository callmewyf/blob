### 整体流程

##### 一、使用*vue-cli*搭建项目框架

##### 二、在项目中增加*serve*文件夹

 ##### 三、新建数据库表

##### 四、项目连接库

> 第一种：使用官方提供的mysql包

这里可采用多种方式，nodejs提供了mysql模块来连接数据库

```
// mysql.createConnection 方法

var connection = mysql.createConnection({
    host: 'localhost',
    port: 3306,
    user: 'root',
    password: '123456',
    database: 'test'
});

// 连接
connection.connect(function (err) {
    if (err) {
        console.log('[query] - :' + err);
        return;
    }
    console.log('[connection connect]  succeed!');
});

// 查询数据
connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});

//关闭连接
connection.end(function (err) {
    if (err) {
        return;
    }
    console.log('[connection end] succeed!');
});
```

当有多连接时，应该使用连接池概念

```
// mysql.createPool 方法

var pool = mysql.createPool({
    host: 'localhost',
    port: 3306,
    user: 'root',
    password: '123456',
    database: 'test'
});
pool.getConnection(function(err, connection) {
    if(err){
        console.log("建立连接失败");
    } else {
        console.log("建立连接成功");
        console.log(pool._allConnections.length); //  1
        connection.query('select * from user', function(err, rows) {
            if(err) {
                console.log("查询失败");
            } else {
                console.log(rows);
            }
            // connection.destory();
            console.log(pool._allConnections.length);  // 0
        })
    }
    pool.end();
})
```

> 第二种：使用ORM(Object-Relational Mapping)框架

ORM就是将关系型数据库映射为实例对象，简化数据库操作

可以选用Node的ORM框架Sequelize来操作数据库

需注意的是，安装Sequelize必须同时安装mysql包，因为Sequelize会用到