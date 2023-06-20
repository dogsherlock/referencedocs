
> 这是一些比较常用的命令, 大家可以复制后用typora做成pdf格式，方便快速查询
> 后续不定期更新




###### 官方文档
```txt
[官方文档](https://git-scm.com/docs/)
本地文档: file:///D:/cmder/vendor/git-for-windows/mingw64/share/doc/git-doc/git-branch.html

Git 本地数据管理，大概可以分为三个区，工作区,暂存区和版本库。

工作区（Working Directory）
是我们直接编辑的地方，例如 Android Studio 打开的项目，记事本打开的文本等，肉眼可见，直接操作。
暂存区（Stage 或 Index）
数据暂时存放的区域，可在工作区和版本库之间进行数据的友好交流。
版本库（commit History）
存放已经提交的数据，push 的时候，就是把这个区的数据 push 到远程仓库了。
```
###### git配置默认存储路径
1. 全局配置 `C:\Users\username\.gitconfig`
```.gitconfig
[user]
	name = username
	email = username@gmail.com
[credential]
	helper = store
[http]
	sslVerify = false
[pull]
	ff = only
[https]
	sslVerify = false
[core]
	autocrlf = true
[push]
	default = current
[http]
    proxy = http://w3账号:w3密码@proxy.huawei.com:8080
[https]
    proxy = https://w3账号:w3密码@proxy.huawei.com:8080
```

2. 项目配置 `项目\.git\config `
* 配置用户名和邮箱
```console 
git config --global user.name "username"
git config --global user.email "email"
```
* 配置公钥
> 使用SSH协议公钥认证
```console
$ ssh-keygen -t rsa -C "username@gmail.com"
# cd %HOME%/.ssh
# 将id_rsa.pub文件中的公钥添加到isource平台
```

3. .gitignore-指定要忽略的未跟踪文件
Git 已经跟踪的文件不受影响, 按照以下优先级从高到低(在一个优先级内，最后匹配的模式决定结果)
```txt
Patterns read from the command line for those commands that support them.

Patterns read from a .gitignore file in the same directory as the path, or in any parent directory (up to the top-level of the working tree), with patterns in the higher level files being overridden by those in lower level files down to the directory containing the file. These patterns match relative to the location of the .gitignore file

Patterns read from $GIT_DIR/info/exclude即.git/info/exclude
git ls-files --others --exclude-from=.git/info/exclude


Patterns read from the file specified by the configuration variable core.excludesFile
git config --global core.excludesFile "some\path\to\.gitignore"
```
4. ssh目录
`cd %HOME%/.ssh`
```console
id_rsa 私钥文件
id_rsa.pub 公钥文件
known_hosts 包含所有远程已经连接过已知的hosts, openssh会核对公钥
```


###### git账号和密码(用户凭据)
1. `C:\Users\username\.git-credentials`

###### 使用git进行版本控制后文件颜色含义
红褐色： 创建之后没有add，没提交，不在版本控制范围之内，这时候文件是红褐色的，需要先add文件
绿色： add之后是文件绿色的，没有提交（commit）
蓝色： 原本有一个文件，改动过后没有提交（commit）是蓝色的，提交之后，变成正常颜色
白色: 工作区的文件


###### 常用命令

1. 配置类
* git访问外网clone需要配置代理
```console
https.proxy=https://username:password@host:port/
git config --global https.proxy https://username:password@host:port/
git config --global  http.proxy http://username:password@host:port/
```
* 如果切回内网仓库下载，需要取消代理
```console
git config --global --unset http.proxy
git config --global --unset https.proxy
```
* 查看是否生效
`git config --global  -l`

* 查看单独的某项配置
`git config --global 属性`
eg:
git config --global user.name

* 配置git push的默认行为
https://git-scm.com/docs/git-config#Documentation/git-config.txt-pushdefault
`git config --global push.default`

* 配置允许文件路径过长
`git config --global core.longpaths true`

* 设置autocrlf为true 
Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF
`git config --global core.autocrlf true`

* 设置autocrlf为input
core.autocrlf设置成input来告诉 Git 在提交时把CRLF转换成LF，签出时不转换
这样会在Windows系统上的签出文件中保留CRLF，会在Mac和Linux系统上，包括仓库中保留LF
`it config --global core.autocrlf input`

* 设置autocrlf为false 
如果你是Windows程序员，且正在开发仅运行在Windows上的项目，可以设置false取消此功能，把回车符记录在库中
`git config --global core.autocrlf false`

* git diff忽略换行符的差异
`git config --global core.whitespace cr-at-eol`


2. 切换类
git clone master分支之后如何切换同步远程的其他分支(命令的方式):
feature分支为需要同步远程的特性分支: `git checkout -b feature origin/feature`
直接同步远程的特性分支: `git checkout -t origin/feature`

本地切换到指定的commit 
`git checkout commitid `
在本地新建一个dev分支 
`git checkout -b dev commitid`

3. 凭据类
查看系统支持哪种credential.helper:
`git help -a | grep credential`
配置credential
`git config --global credential.helper store`

4. 冲突类
```console
git pull = git fetch + git merge 
git pull --rebase = git fetch + git rebase 
git pull origin master --rebase 
```
现在有两个分支：test和master，假设远端的master的代码已经更改了（在B基础上变动：C,F），
test的代码更改了要提交代码（在B基础上变动：D,E），如下图：
```
      D---E test
      /
 A---B---C---F--- master
```
问题就来了，如果C,F和D,E的更改发生冲突，那么就需要我们合并冲突了，
下面我们来看看git merge和git rebase怎么合并的

`git merge:`
```
       D--------E
      /          \
 A---B---C---F----G---   test, master
```

`git rebase:`
```
A---B---D---E---D‘---F‘---   test, master
```

对比可看出：git merge多出了一个新的节点G，会将远端master的代码和test本地的代码
在这个G节点合并，之前的提交会分开去显示。git --rebase会将两个分支
融合成一个线性的提交，不会形成新的节点。

> rebase的优点

想要更好的提交树，使用rebase操作会更好一点。
这样可以线性的看到每一次提交，并且没有增加提交节点。
merge 操作遇到冲突的时候，当前merge不能继续进行下去。手动修改冲突内容后，add 修改，commit 就可以了。
而rebase 操作的话，会中断rebase,同时会提示去解决冲突。
解决冲突后,将修改add后执行git rebase –continue继续操作，或者git rebase –skip忽略冲突

###### git diff
```console
[git diff](https://git-scm.com/docs/git-diff)
git diff branch1 branch2显示出所有有差异的文件的详细差异
git diff branch1 branch2 --stat显示出所有有差异的文件列表
git diff branch1 branch2 file显示指定文件的详细差异
```

顺序: 工作区、暂存区、版本库
* 工作区修改之后还没有暂存起来的变化内容(工作区vs暂存区)
`git diff `
* 暂存起来的文件和上次提交时的快照之间的差异(暂存区vs版本库)
`git diff --cached或git diff --cached --stat
* 工作区vs版本库
`git diff head`
* 只显示修改的名字
git diff --name-only
* 显示某个文件的变化
git diff --stat filename






###### git show
```console 
git show [<options>] [<object>…​]
# 显示各种类型的对象
git-show: show various types of objects(blobs, trees, tags and commits)

Shows one or more objects (blobs, trees, tags and commits).
# 对于提交，显示日志消息和文本差异
For commits it shows the log message and textual diff. It also presents the merge commit in a special format as produced by git diff-tree --cc.
# 对于标签，它显示标签消息和引用对象
For tags, it shows the tag message and the referenced objects.
# 对于树, 显示名称 
For trees, it shows the names (equivalent to git ls-tree with --name-only).
# 对于简单blobs, 它显示普通的文本
For plain blobs, it shows the plain contents.

The command takes options applicable to the git diff-tree command to control how the changes the commit introduces are shown.

This manual page describes only the most frequently used options.
```

* 显示提交详情
`git show commitid`

* 显示某次提交某个文件的变动
`git show commitid filename `

* 显示某次合并提交(merge commit)的变动
```console 
# 三种方法
1. git show -m commitid 
2. git show --first-parent commitid 
3. git diff commitid^ commitid
```

###### git rm
删除暂存区或分支上的文件，同时工作区不需要这个文件
`git rm filename`
当需要删除暂存区或分支上的文件,同时工作区需要这个文件,但是不需要被版本控制
`git rm --cache filename `


###### git restore
> 把文件从缓存区撤销,回到未被追踪的状态

* git restore filename 
撤销本地修改, 将不在暂存区的文件撤销更改(git status提示的被修改但未被加入暂存区的内容，会被撤销)
* git restore --staged filename 
撤销暂存区 将文件从暂存区撤出，但不会撤销工作区文件的更改(git status 提示的被修改且被加入暂存区
的内容,会被撤销，工作区文件的更改不会变)

###### git-reset - Reset current HEAD to the specified state
> 有三种模式 soft  mixed hard
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存失败,源站可能有防盗链机制,建议将图片保存下来直接上传下上传(irCSIGme4NZU-5165609402136)(./git_learning_sources/git_reset.png)(./git_learning_sources/git_reset.png)]

###### remote repo usage
* 查看远程仓库
`git remote -v `
* 删除远程仓库
`git remote rm <shortname>`
* 添加远程仓库
`git remote add <shortname> <url>`
* 查看某一个远程仓库的更多信息
`git remote show <remote>`
eg:
```console
git remote show origin 
git remote show upstream 
```
* 修改一个远程仓库的简写名
如将pb重命名位paul
`git remote rename pb paul`


###### git fetch 从远程仓库下载数据到本地
```console
[git fetch](https://git-scm.com/docs/git-fetch)
--all: Fetch all remotes
```

* 从仓库获取所有分支或者指定分支
```console
git fetch <remote>
git fetch <remote> <branch>
```
* 获取所有已注册远程分支
`git fetch --all`

###### 分支相关
* 修改本地或远程分支名称
```console
1. 先将本地分支重命名
git branch -m <old> <new>
2. 删除远程分支
git push --delete origin 
3. 将重命名后的分支推到远端 
git push origin new
4. 把修改后的本地分支与远程分支关联
git branch --set-upstream-to origin/newBranch
```

* 基于某个分支创建新的分支
`git checkout -b yourbranchname origin/oldbranchna.`

###### commit
* 合并多条commit 
```console
1. git log 查看所有的git 记录
2. git rebase -i commitId
这里的commitId指的是想要合并的最后一条的下一条记录也就是
3. 修改关键字
pick 的意思是要会执行这个 commit
squash 的意思是这个 commit 会被合并到前一个commit
```
###### git stash
* 将当前分支未提交的修改切换到另一个分支

方法一：
```console
git stash
git checkout branch2 ~
git stash pop
```

方法二：
```console
git stash
git checkout branch2
git stash list ＃检查在不同分支中创建的各种存储
git stash apply x ＃选择正确的一个
注意：git stash apply，会将当前分支的最后一次缓存的内容释放出来
```