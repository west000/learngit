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
```

# 添加文件到git仓库
```
$ vim readme.md
$ vim git_manual.md
$ git add readme.md git_manual.md 	# 一次性添加多个文件
$ git commit -m "add file"		# 提交
```




