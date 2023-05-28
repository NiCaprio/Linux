# git操作
# git操作
##设置用户签名
~~~
git config --global user.name 名字
git config --global user.email 邮箱
~~~
##初始化本地库
需要进入到你要初始化的目录
~~~
git init
~~~
##创建一个文本文件
~~~
vim hello.txt
~~~
查看状态
~~~
git status
~~~
发现hello.txt变红

添加到暂存区
~~~
git add hello.txt
~~~
查看状态发现hello.txt变绿色
删除暂存区的hello.txt
~~~
git rm --cached hello.txt
~~~
只是删除了暂存区的文件，本地文件(工作区)并没有删除

##提交本地库
~~~
git commit -m "版本信息" hello.txt
~~~
查看状态发现显示
On branch master
nothing to commit, working tree clean

查看日志
~~~
git reflog //精简
git log //完整
~~~
发现指针指向master


##版本穿梭
~~~
git reset --hard 版本号
~~~


##与GitHub的操作
创建别名
~~~
git remote add 别名 仓库地址
~~~
推送到远程库
~~~
git push 别名 分支
~~~




















