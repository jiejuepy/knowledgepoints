### git操作
克隆代码 get clone https网址

git branch 不加名字就是查看分支，加名字就是创建分支

git checkout -b 分支名 切换到分支，或者分支不存在就创建分支

git branch -D 分支名 删除本地分支，在其他分支里面执行

git push origin --delect 分支名  删除远程的分支

touch name 创建文件名

vim name 编辑文件

rm name 删除文件

git add . 添加修过的所有文件加到缓存区

git add name1 name2 .... 添加修改的单个或多个文件到缓存区

git status 查看状态

git config --global user.email

git config --global user.name

合并add和commit操作

git commit -am'注解'

创建秘钥

ssh-keygen -t rsa -C xxxx@qq.com 账号

git diff 分支1 分支2 对比2个分支是否相同

get merge name1 name2 分支合并，每个分支里面的修改要记得push

### 版本号操作
git tag -a 版本号 -m 注解

git push origin 版本号 上传版本号

git tag -d 版本号 本地删除版本号

git push origin --delete 版本号  删除远程的版本号

git stash 缓存本地修改的代码

git stash list 查看缓存列表

git stash apply @{x} 释放编号为x的缓存，但该缓存还是在list中

git log 查看日志

git reset 版本号 就可以返回之前的版本

