# git的基本操作
## 安装git
```sh
$ sudo yum install git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 创建版本库
```sh
$ mkdir learngit
$ cd learngit
$ git init	# 初始化一个git仓库

$ ls -a
. .. .git	# .git就是git的版本库，这里面的文件很多，但有个重要的叫暂存区（index或者stage）
		# learngit这个目录就叫做工作区
```

## 添加文件到git仓库
```sh
$ vim readme.md
$ vim git_manual.md
$ git add readme.md git_manual.md 	# 一次性添加多个文件
$ git commit -m "add file"		# 提交
```

## 版本回退
```sh
$ git reset --hard HEAD^	# 回退到上一个版本
$ git reset --hard HEAD^^	# 回退到上上个版本
$ git reset --hard HEAD~100	# 回退到前100个版本

$ git reset --hard commit_id	# 回退到指定的版本号commit_id

$ git log	# 查看提交的历史，以便确定回退到之前哪个版本
$ git reflog	# 查看命令的历史，以便确定回退到未来哪个版本
```

## 工作区和暂存区
```sh
$ git add readme.md		# git add 将文件的修改从工作区添加到暂存区中
$ git commit 			# git commit 将暂存区的内容提交到当前分支
$ git reset HEAD file		# 把暂存区中的修改撤销掉(unstage)，重新放回工作区，这里使用HEAD表示最新版本
				# git reset既可以回退版本，也可以把暂存区的修改回退到工作区 
```

## 撤销修改
```sh
$ git checkout -- readme.md	# 将readme.md在工作区的修改全部撤销，这里有两种情况
				# readme.md自修改后还没有放到暂存区中，现在撤销修改就回到和版本库一样的状态
				# readme.md已经加入暂存区了，然后又被修改，现在撤销修改就回到添加到暂存区后的状态
				# 间而言之，就是让readme.md回到最近一次git commit或git add时的状态

$ git reset HEAD readme.md	# 把暂存区的修改撤销掉（unstage），重新放回工作区

$ git commit 
$ git reset --hard HEAD^	# 如果已经将暂存区的内容提交到当前分支，则只能通过版本回退进行撤销了，不过前提是未提交到远程仓库
```

## 删除文件
```sh
$ vim test
$ git add test	# 将test添加到暂存区中
$ rm test	# 删除工作区的test
$ git rm test	# 如果确实需要删除test，则使用将test从暂存区中删除
$ git checkout -- test	# 如果是误删了，可以撤销工作区的操作
```

# 远程仓库
## 使用github作为远程仓库
本地git仓库和github仓库之间的传输是通过SSH加密的，因此需要以下设置

1. 创建SSH Key：在用户主目录下（/home/west）,查看是否有.ssh目录，如果没有，使用以下命令创建
```sh
$ ssh-keygen -t rsa -C "email@example.com"
$ cd .ssh
$ ls 
$ id_rsa id_rsa.pub	# 前者是私钥，后者是公钥
``` 
一路回车下去就可以了，这里可以不用为key设置密码

2. 登录github账号，Settings->SSH and GPG keys->New SSH key->输入key的Title(随便起个名字即可)，然后将公钥的内容粘贴到Key中即可

## 在github中添加远程仓库
1. 登录github账号，右上角“+”号->New repository->输入Repository name->Create repository->创建仓库成功之后，跳转到一个指导页面：
```sh
Quick setup — if you’ve done this kind of thing before
[Set up in Desktop] or [HTTPS SSH git@github.com:west000/learngit.git]
We recommend every repository include a README, LICENSE, and .gitignore.

…or create a new repository on the command line
echo "# learngit" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:west000/learngit.git
git push -u origin master

…or push an existing repository from the command line
git remote add origin git@github.com:west000/learngit.git
git push -u origin master

…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.
```

目前这个远程仓库空空如也，github告诉我们有三种方案：
- 从这个仓库克隆出新的仓库
- 把一个已有的本地仓库与之关联，然后把本地仓库的内容推送到github仓库
- 从其它仓库导入代码到本仓库

2. 将本地仓库与远程仓库进行关联
```sh
$ git remote add origin git@github.com:west000/learngit.git	# 关联远程库origin
$ git push -u origin master	# 将本地库master分支的所有内容推送到远程库origin的master分支中
				# 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，
				# Git不但会把本地的master分支内容推送的远程新的master分支，
				# 还会把本地的master分支和远程的master分支关联起来，
				# 这样在以后的推送或者拉取时就可以简化命令

$ ...... # 在本地库做一些修改，并提交
$ git push origin master	# 把本地master分支的最新修改推送到远程库
```
上面的origin就是远程库的名字，当然可以改成其他名字，不过远程库默认就是这个名字

## 从远程库克隆
```sh
$ mkdir /home/west/learngit_clone
$ cd /home/west/learngit_clone
$ git clone git@github.com:west000/learngit.git
$ cd learngit
$ ls -a
$ .  ..  .git  git_manual.md  readme.md
```
git支持多种协议，包括https，但是通过ssh支持的原生git协议速度是最快的

# 分支管理
## 创建与合并分支
- 查看分支：git branch
- 创建分支：git branch <name>
- 切换分支：git checkout <name>
- 创建+切换分支：git chechout -b <name>
- 合并某分支到当前分支：git merge <name>
- 删除分支：git branch -d <name>

## 解决冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

```sh
$ git merge dev 	# 将dev分支合并到当前分支
			# 如果合并的时候有冲突，会在当前分支的冲突文件中，显示下列内容
			# <<<<<<< HEAD	# HEAD指向当前分支
			# 本分支冲突文件的冲突内容
			# =======	# 分支的分隔符
			# dev分支冲突文件的冲突内容
			# >>>>>>> dev	# dev分支

# 修改冲突的方式：在当前的冲突文件中解决冲突，然后在进行`git add <file>; git commit -m "conflict fixed"`即可。
# 注意：这样解决冲突，dev分支上的内容是不会改变的！我们合并分支的时候，解决完冲突，往往会删除dev分支或者将dev分支的更新为master分支的最新内容
```
合并完分支后，可以使用`git log --graph`查看分支的合并图
```sh
git log --graph --pretty=oneline --abbrev-commit
```

## 分支管理策略
实际开发中，不会直接在master进行开发。master分支应该是非常稳定的，仅仅用来发布新版本，平时不能在上面干活。通常情况下，会在远程仓库新创建一个dev分支，在dev分支上干活，等到要发布版本的时候，再将dev分支合并到master分支上。每个人都在dev上干活，并且每个人都有自己的分支，时不时往dev上合并。

默认情况下，`git merge dev`使用`fast forward`的方式进行合并，这种形式只是简单调整指针而已，因此速度很快，但是合并分支之后，如果我们删除掉dev分支，在这种模式下dev分支的信息将丢失。
为了避免这个问题，可以在合并时使用`--no-ff`禁用`fast forward`模式，这时git会生成一个新的commit，这样从历史上就可以看到dev分支信息（即使我们删除了dev）

```sh
$ git merge --no-ff -m "merge with no-ff" dev	# 使用--no-ff会产生一次commit，因此需要加上-m提供提交信息
$ git branch -d dev					# 现在即使删除了dev分支
$ git log --graph --pretty=oneline --abbrev-commit	# 仍然能够查看到dev分支的历史
```

## Bug分支
修复bug时，可以通过创建新的bug分支(命名如issue-101), 进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。

```sh
$ git stash			# 保存当前工作区
$ git stash list		# 查看工作区保存的位置
$ git stash pop			# 恢复最新保存的工作区，并将其从stash中删除
$ git stash apply [stash@{0}]	# 恢复指定的工作区，默认是最新保存的工作区
$ git stash drop [stash@{0}]	# 删除指定的工作区，默认是最新保存的工作区
```

## Feature分支
开发一个新feature，最好新建一个分支(命名如feature-101)
如果要丢弃一个没有被合并过的分支，如果使用`git branch -d <name>`将会报错，此时可以通过`git branch -D <name>`强行删除。

## 多人协作
多人协作的工作模式通常如下：
- 首先，可以试图用`git push origin branch-name`推送自己的修改；
- 如果推送失败，则因为远程分支比本地分支更加新，需要先用`git pull`拉取远程分支，并试图与本地分支合并；
- 如果合并有冲突，则解决冲突，并在本地提交；
- 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功

值得注意的：如果`git pull`提示“no tracking information for the current branch”，则说明本地分支和远程分支的链接关系没有创建，用命令`git pull <remote> <branch>`指明拉取的远程分支，或在当前分支使用`git branch --set-upstream-to=origin/<branch-name> current-branch`与远程分支建立链接。

```sh
$ git remote		# 查看远程库名称
$ git remote -v		# 查看远程库详细信息，显示可以抓取和推送的远程库的地址

$ git push origin branch-name	# 将本地分支推送到远程分支
				# 如果没有对应的远程分支，则会在远程库创建之
				# 如果有对应的远程分支，且推送失败，则先git pull拉取关联的远程分支，与本地分支进行合并

$ git pull			# 拉取关联的远程分支
$ git pull origin branch-name	# 拉取指定的远程分支  

$ git checkout -b branch-name origin/branch-name # 在本地创建和远程分支对应的分支, 并进行关联

# 下面的两步与上一条命令效果一致
$ git checkout -b branch-name 	# 在本地创建分支
$ git branch --set-upstream-to=origin/<branch-name> current-branch-name # 建立关联 
```

# 标签管理
标签可以看做是对commit-id的易记标识，默认只存储在本地，不会自动推送到远程

## 创建标签
```sh
$ git tag <name> [commit-id]			# 创建一个标签name，commit-id默认为HEAD
$ git tag -a <name> -m "some message"		# 创建一个标签，并指定标签信息
$ git tag -s <name> -m "some message"		# 创建一个标签，指定标签信息，并使用PGP签名标签信息
$ git tag					# 查看所有标签
```

## 操作标签
```sh
$ git push origin <tagname>	# 标签默认只存储在本地，使用该命令可以将指定标签推送到远程库
$ git push origin --tags	# 推送所有未推送的标签到远程库
$ git tag -d <tagname>          # 在本地删除一个标签
$ git push origin :refs/tags/<tagname>	# 在远程库删除一个标签

$ git tag v0.1			
$ git push origin v0.1			# 将标签v0.1推送到远程库
$ git tag -d v0.1			# 在本地删除标签v0.1
$ git push origin :refs/tags/v0.1	# 想要删除远程库的标签，须使用该命令
```

# 自定义git
git提供了许多配置项，可以根据喜好进行定制

## 忽略特殊文件
```sh
$ vim .gitignore	# 创建.gitignore文件，并加入一些过滤规则，如*.txt
$ git add .gitignore	# 必须将.gitignore文件加入到版本库
$ git commit -m "add .gitignore"

$ git add hello.txt	# 由于.gitignore文件中有*.txt的过滤规则，因此该文件无法加入版本库
$ git add -f hello.txt	# 若要强制加入，添加-f选项
$ git check-ignore -v hello.txt		# 查看那个过滤规则阻止了hello.txt的加入
.gitignore:1:*.txt      hello.txt	# 原来是第一条规则阻止了
```

忽略文件的常用原则如下：
- 忽略操作系统自动生成的文件，比如缩略图等
- 忽略编译生成的中间文件、可执行文件等
- 忽略带有敏感信息的配置文件，比如存放口令的配置文件

## 配置别名
```sh
$ git config --global alias.st status	# 为status创建别名st
$ git config --global alias.br branch
$ git config --global alias.co checkout
$ git config --global alias.ci commit

$ git config --global alias.unstage 'reset HEAD'	# 将暂存区的修改撤销掉，重新放回工作区
$ git config --global alias.last 'log -1'	# 显示最后一次提交信息
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
$ git config --global core.quotepath false # 解决git status不能显示中文的问题 
```

值得注意的：
- 配置git的时候，加上`--global`是针对当前用户起作用的，如果不加，则只针对当前的仓库起作用
- 每个仓库的git配置文件放在.git/config文件中
- 当前用户的git配置文件则放在用户主目录中的.gitconfig文件中
- 如果要删除别名，则可以在对应的配置文件中直接删除

## 搭建git服务器


# 常见问题
## git签出远程分支问题
```sh
$ git checkout -b dev origin/dev	# 创建dev分支，并关联远程分支
$ git checkout --track origin/dev	# 已存在的本地分支进行关联
```
如果出现错误
```sh
fatal: Cannot update paths and switch to branch 'dev' at the same time.  
Did you intend to checkout 'origin/dev' which can not be resolved as commit?  
```
可以采用：
```sh
$ git fetch
$ git checkout -b dev origin/dev
```

## 想要忽略项目中的所有可执行文件
Linux中，可执行文件（ELF格式）没有后缀名，想要忽略项目中的所有可执行文件，有几种解决方案：
- 开源项目的常见做法：将所有编译生成的文件（包括可执行文件和中间步骤的目标文件等），都放在build目录下，然后在.gitignore中忽略整个build目录

- 将所有可执行文件的文件名罗列出来，放在.gitignore中（不推荐）
```sh
$ find 目录名 -type f -exec file {} + | grep "ELF*" | sed 's/^.\///g' | awk '{print $1}' | sed 's/://g' >> .gitignore
# 把目录名中的ELF文件追加到.gitignore中
```

- 先把所有文件和目录忽略 -> 再把直接要管理的目录和子目录加进去 -> 接着增加需要管理的文件类型（一般用扩展名） -> 最后把一些特殊的文件处理一下
整个.gitignore文件的内容大致如下：
```sh
#忽略所有文件和目录
*

#增加指定目录和下面所有目录
!/dir1/
!/dir1/**/
!/dir2/
!/dir2/**/

#增加指定扩展名文件和Makefile文件
!*.cpp
!*.c
!*.h
!Makefile

#忽略特殊文件，一般是当前目录下的文件(当前目录不能忽略)
/source.cpp
/source.h
```

