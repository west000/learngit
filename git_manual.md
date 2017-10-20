# 安装git
```
$ sudo yum install git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

# 创建版本库
```
$ mkdir learngit
$ cd learngit
$ git init	# 初始化一个git仓库

$ ls -a
. .. .git	# .git就是git的版本库，这里面的文件很多，但有个重要的叫暂存区（index或者stage）
		# learngit这个目录就叫做工作区
```

# 添加文件到git仓库
```
$ vim readme.md
$ vim git_manual.md
$ git add readme.md git_manual.md 	# 一次性添加多个文件
$ git commit -m "add file"		# 提交
```

# 版本回退
```
$ git reset --hard HEAD^	# 回退到上一个版本
$ git reset --hard HEAD^^	# 回退到上上个版本
$ git reset --hard HEAD~100	# 回退到前100个版本

$ git reset --hard commit_id	# 回退到指定的版本号commit_id

$ git log	# 查看提交的历史，以便确定回退到之前哪个版本
$ git reflog	# 查看命令的历史，以便确定回退到未来哪个版本
```

# 工作区和暂存区
```
$ git add readme.md		# git add 将文件的修改从工作区添加到暂存区中
$ git commit 			# git commit 将暂存区的内容提交到当前分支
$ git reset HEAD file		# 把暂存区中的修改撤销掉(unstage)，重新放回工作区，这里使用HEAD表示最新版本
				# git reset既可以回退版本，也可以把暂存区的修改回退到工作区 
```

# 撤销修改
```
$ git checkout -- readme.md	# 将readme.md在工作区的修改全部撤销，这里有两种情况
				# readme.md自修改后还没有放到暂存区中，现在撤销修改就回到和版本库一样的状态
				# readme.md已经加入暂存区了，然后又被修改，现在撤销修改就回到添加到暂存区后的状态
				# 间而言之，就是让readme.md回到最近一次git commit或git add时的状态

$ git reset HEAD readme.md	# 把暂存区的修改撤销掉（unstage），重新放回工作区

$ git commit 
$ git reset --hard HEAD^	# 如果已经将暂存区的内容提交到当前分支，则只能通过版本回退进行撤销了，不过前提是未提交到远程仓库
```

# 删除文件
```
$ vim test
$ git add test	# 将test添加到暂存区中
$ rm test	# 删除工作区的test
$ git rm test	# 如果确实需要删除test，则使用将test从暂存区中删除
$ git checkout -- test	# 如果是误删了，可以撤销工作区的操作
```



