# Git管理区
![](git_1.png)
Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库

***

# 规范提交信息

示例
`fix(test) : change contant`

***

# 基本操作

[菜鸟教程参考](https://www.runoob.com/git/git-basic-operations.html)

```
git init

git clone

git add

git status -s  查看仓库当前的状态，显示有变更的文件。(-s获得较短结果)

git diff    比较文件的不同，即暂存区和工作区的差异。

git commit -m "first commit"

git reset [--soft | --mixed | --hard] [HEAD]

git rm      将文件从暂存区和工作区中删除。

git mv      移动或重命名工作区文件。

git log

```

***

# 远程操作

```
git remote     远程仓库操作(详见补充说明)

git fetch      从远程获取代码库

git pull

git push
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin master     将本地的 master 分支推送到 origin 主机的 master 分支。

git fetch upstream    更新本地版本
```

***

# 分支管理

```
git branch (branchname)     创建分支

git checkout (branchname)     切换分支

git merge                 合并分支
```

## 列出分支

`git branch`

## 删除分支

`git branch -d (branchname)`

## 分支合并

`git merge`

```
$ git branch
* master
  newtest
$ ls
README        test.txt
$ git merge newtest
Updating 3e92c19..c1501a2
Fast-forward
 runoob.php | 0
 test.txt   | 1 -
 2 files changed, 1 deletion(-)
 create mode 100644 runoob.php
 delete mode 100644 test.txt
$ ls
README        runoob.php
```

以上实例中我们将 newtest 分支合并到主分支去，test.txt 文件被删除。

## 合并冲突

```
合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。

$ git branch
* master
$ cat runoob.php

首先，我们创建一个叫做 change_site 的分支，切换过去，我们将 runoob.php 内容改为:

<?php
echo 'runoob';
?>

创建 change_site 分支：

$ git checkout -b change_site
Switched to a new branch 'change_site'
$ vim runoob.php
$ head -3 runoob.php
<?php
echo 'runoob';
?>
$ git commit -am 'changed the runoob.php'
[change_site 7774248] changed the runoob.php
 1 file changed, 3 insertions(+)
 

将修改的内容提交到 change_site 分支中。 现在，假如切换回 master 分支我们可以看内容恢复到我们修改前的(空文件，没有代码)，我们再次修改 runoob.php 文件。

$ git checkout master
Switched to branch 'master'
$ cat runoob.php
$ vim runoob.php    # 修改内容如下
$ cat runoob.php
<?php
echo 1;
?>
$ git diff
diff --git a/runoob.php b/runoob.php
index e69de29..ac60739 100644
--- a/runoob.php
+++ b/runoob.php
@@ -0,0 +1,3 @@
+<?php
+echo 1;
+?>
$ git commit -am '修改代码'
[master c68142b] 修改代码
 1 file changed, 3 insertions(+)

现在这些改变已经记录到我的 "master" 分支了。接下来我们将 "change_site" 分支合并过来。

$ git merge change_site
Auto-merging runoob.php
CONFLICT (content): Merge conflict in runoob.php
Automatic merge failed; fix conflicts and then commit the result.

$ cat runoob.php     # 打开文件，看到冲突内容
<?php
<<<<<<< HEAD
echo 1;
=======
echo 'runoob';
>>>>>>> change_site
?>

我们将前一个分支合并到 master 分支，一个合并冲突就出现了，接下来我们需要手动去修改它。

$ vim runoob.php 
$ cat runoob.php
<?php
echo 1;
echo 'runoob';
?>
$ git diff
diff --cc runoob.php
index ac60739,b63d7d7..0000000
--- a/runoob.php
+++ b/runoob.php
@@@ -1,3 -1,3 +1,4 @@@
  <?php
 +echo 1;
+ echo 'runoob';
  ?>

在 Git 中，我们可以用 git add 要告诉 Git 文件冲突已经解决

$ git status -s
UU runoob.php
$ git add runoob.php
$ git status -s
M  runoob.php
$ git commit
[master 88afe0e] Merge branch 'change_site'

现在我们成功解决了合并中的冲突，并提交了结果。
```

***

# 文件状态

A: 你本地新增的文件（服务器上没有）.

C: 文件的一个新拷贝.

D: 你本地删除的文件（服务器上还在）.

M: 文件的内容或者mode被修改了.

R: 文件名被修改了。

T: 文件的类型被修改了。

U: 文件没有被合并(你需要完成合并才能进行提交)。

X: 未知状态(很可能是遇到git的bug了，你可以向git提交bug report)。

***

# git与github连接

```
git config --global user.name "your name"     设置用户名

git config --global user.email "your email"    设置邮箱

ssh-keygen -t rsa -C "your email"         生成ssh公钥

ssh -T git@github.com          测试连接
```

***

# 相关命令补充说明

## git remote 

`git remote -v          显示所有远程仓库`

`git remote show [remote]       显示某个远程仓库的信息：`

`git remote add [shortname] [url]        添加远程版本库`

`git remote rm [name]      删除远程仓库`

`git remote rename [old_name] [new_name]      修改仓库名`

