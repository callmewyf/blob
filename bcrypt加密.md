##### 使用方法

> 场景：

在数据库中存储的密码应该是经过`bcrypt`加密的，而表单提交

的密码都是明文，在后端`controller`中如何对比？

> 解答：

使用`bcrypt.compareSync`方法

`bcrypt.compareSync('表单提交密码', '数据库密码')`

> 原理：

+ 加密时，先随机生成`salt`，`salt`跟`password`进行`hash`

+ 对比时，先从`hash`中取出`salt`，再跟表单`password`进行`hash`，`compareSync`中实现了这个过程
+ `bcrypt.compareSync(password, hashFromDB)`



