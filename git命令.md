# git使用说明

可使用命令行操作，也可使用source tree图形化界面来操作

### 一、git全局设置

###### 设置用户名和密码

```
git config --global user.name "xxx"
git config --global user.email "xxx@xx.com"
```

### 二、创建新版本库

###### 将线上的库拷贝到本地

```
git clone 仓库地址
```

也可以拷贝分支

```
git clone -b 分支名称 仓库地址
```

###### 分支管理

```
git branch                                   // 查看本地分支
git branch -a                                // 查看本地分支及远程分支
git branch -v                                // 查看分支详情，包括commitId及提交信息

git branch 分支名                             // 新建分支

git checkout 分支名                           // 切换到指定分支
git checkout -b 分支名                        // 创建并切换到指定分支
git checkout -b 本地分支名 origin/远程分支名     // 拉取远程分支并创建本地分支

git branch -d 分支名                          // 删除分支
                                            // 注：删除分支前需要先切换到其它分支才能进行删除操作

git branch -m 分支名 新分支名                   // 重命名分支

git diff 远程分支名                            // 比较本地仓库与远程分支区别
git merge 远程分支名                           // 将指定分支合并到当前分支

// 分支恢复
// 对于已经有提交记录的分支删除后，实际上只是删除指针，commit记录还保留
git reflog                                   // 查找该分支指向的commitId
git branch 分支名 commitId                    // 根据指定的commitId创建新分支
```

###### 远程

```
// 连接远程库
git remote add origin http://10.3.23.95:8088/hht/hht-bigscreen.git
// 查看远程库
git remote -v
// 删除连接
git remote rm origin
// 更改连接名称
git remote rename origin old-origin
```

###### 提交内容

```
git add 文件名                      // 添加指定修改文件
git add *                          // 添加所有修改文件
git add .                          // 添加所有修改文件

git commit -m "说明"                // 添加说明 生成本地版本

git push -u origin 远程分支名        // 将本地分支推送到远程分支上
git push origin 本地分支名:远程分支名  // 将本地分支推送到远程分支上
```

###### 本地已有文件夹关联仓库

```
// 进入文件夹
git init
git remote add origin 仓库地址
```

###### 迁徙已存在的仓库

```
git remote rename origin old-origin   // 重命名
git remote add origin 仓库地址         // 重新连接新地址

git remote -v   // 查看远程分支主机名及地址
```

###### 拉取内容

```
git pull 远程主机名 远程分支名:本地分支名
eg: git pull origin master:develop   // 将远程主机origin的master分支拉取过来，与本地的develop分支合并

git fetch 和 git pull
// 都会拉取到本地，fetch不会自动merge，pull会自动merge

// 在这个阶段会遇到冲突，手动解决冲突，重新add
```

###### 遇到问题解决问题

```
 // 没有pull的情况下直接push了（期间有其他人提交），会报错本地和远程分支有分歧
 // 强制push到origin上游
 git push origin v1.0 -f
```

###### 回退

```
// 不想要本地修改了怎么办

// 一、未使用 git add 缓存时
git checkout -- filepathname   // 不要忘记中间的 “--” ，不写就成了检出分支了！
// 放弃所有文件修改
git checkout .

// 二、使用了 git add 缓存了代码
git reset HEAD filepathname
// 放弃所有缓存
git reset HEAD .
// 相当于撤销 git add 命令所做的工作
// 在使用本命令后，本地的修改并不会消失，而是回到了如（一）所示的状态
// 继续用（一）中的操作，就可以放弃本地的修改

// 三、已经用 git commit 提交了代码
git reset --hard HEAD^   // 回退到上一次commit的状态
git reset --hard commitid   // 回退到任意版本
git log   // 查看git的提交历史，查看commitid
```

合并两个分支

```
// 保证在同一远程库下，保证开发本地库的代码全部提交到远程库，保持最新
// 在需要执行合并的本地库下
git pull origin(远程主机) master(远程分支)
// 返回的信息有需要merge的
// 先将冲突放入缓存区
git stash
// 重新拉取，同时会自动merge没有冲突的部分
git pull origin(远程主机) master(远程分支)
// 释放冲突
git stash pop
// 去编译器中手动合并冲突
// 提交修改
git add . 
git commit -m"merge"
git push origin(远程主机) master(远程分支)
// 此时远程库就是两个合并后的新代码
// 在开发本地库中拉取
git pull origin(远程主机) master(远程分支)
```

