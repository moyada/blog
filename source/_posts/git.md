---
title: Git的学习
categories: git
date: 2017-02-25 12:53:10
tags:
---

# 设置基本配置
* 让Git显示颜色，会让命令输出看起来更醒目：
```
$ git config --global color.ui true
```

* 设置全局用户信息
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

* 记录用户密码
```
git config credential.helper store
# 设置过期时间
git config credential.helper 'cache --timeout=3600'
```

* 增加删除信息
```
$ git config --list --global
$ git config --global --add user.name xyk
$ git config --get user.name
$ git config --global --unset user.name xyk
```

* 设置命令别名
```
设置checkout别名为co
$ git config --global alias.co checkout
设置stash别名为sh
$ git config --global alias.co checkout
```

# 简单使用
1. 初始化一个Git仓库，可以使用$ git init命令。
或者$ git clone [-b branch_name] "repository_url" 从远程仓库拉取
2. 添加文件到Git仓库，分两步：
第一步，使用命令$ git add <file> 添加文件到暂存区里。
第二步，使用命令$ git commit [-a] -m "message"，将暂存区的文件提交到本地仓库里。
要随时掌握工作区的状态，使用git status命令。
如果$ git status告诉你有文件被修改过，用$ git diff可以查看修改内容。
3. 如果需要丢弃未暂存的文件，可以使用$ git checkout [--] <file> 丢弃工作区的文件
4. 如果需要丢弃已暂存的文件，可以使用$ git reset [HEAD] <file> 丢弃暂存区的文件
移除文件: $ git rm --cached <file>
移动重命名文件: $ git mv <file> <new_file>
将暂存区的文件添加到本地仓库里的最后一次提交: $ git commit --amend
5. $ git push origin <branch_name> 提交到远程仓库
6. 如果本地仓库与远程仓库不属于同一祖先的话，先git pull <remote_name> <branch> --allow-unrelated-histories 同步分支后在提交

# 忽略文件
如果有不需要参与提交的文件，可以创建文件.gitignore，以正则表达式的形式，将不需要的文件表达加入进去:

| 表达式  | 描述      |
|--------|----------|
|*~  | 将vim中间文件，排除git管理|
|*.class  | 将以class结尾的文件，排除git管理|
|*.[ab]   | 将以a或b结尾的文件，排除git管理|
|target/   | 将第一级的target文件夹，排除git管理|
|**/log   | 将所有的log文件夹，排除git管理|
|!git.class   | 将git.class文件纳入到git管理|

# 分支
```
查看分支：$ git branch
创建分支：$ git branch <branch_name>
切换分支：$ git checkout <branch_name>
创建+切换分支：$ git checkout -b <branch_name>
删除分支：$ git branch -d <branch_name>
```
如果要丢弃一个没有被合并过的分支，可以通过$ git branch -D <branch_name>强行删除。

# 贮藏工作现场
Git提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
```
$ git stash save [-a "stash_name"]
```
用$ git stash list命令可以查看所有工作现场。
恢复有两个办法：
* 一是用$ git stash apply恢复，但是恢复后，stash内容并不删除，你需要用$ git stash drop <stash_name>来删除；
* 另一种方式是用$ git stash pop，恢复的同时把stash内容也删了。
当想要一次性清理全部stash时，可以通过命令$ git stash clear来完成。

# 标签
标签可以用于给分支打上多个tag，作为版本的标明
从版本tag上新建分支bug分支，用于修复历史版本存在bug。

* 在Git中打标签非常简单，首先，切换到需要打标签的分支上，然后通过命令
```
$ git tag [tagname]
```
新建一个标签，默认为HEAD，也可以指定一个tag name。

* 要对历史提交打标签，则需要在后面添加对应的commit id，敲入命令：
```
$ git tag <tagname> <commit_id>
```

* 还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
```
$ git tag -a <tagname> -m "version 0.1 released" 3628164
```

* 还可以通过-s用私钥签名一个标签：
$ git tag -s <tagname>  -m "signed version 0.2 released" fec145a
签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错

查看所有标签可以用命令: $ git tag

查看标签信息可以用: $ git show <tagname>

命令$ git push origin <tagname>可以推送一个本地标签；
命令$ git push origin --tags可以推送全部未推送过的本地标签；
命令$ git tag -d <tagname>可以删除一个本地标签；
命令$ git push origin :refs/tags/<tagname>可以删除一个远程标签。

# 合并
在需要合并某分支到当前分支时:
```
$ git merge <branch_name>
```

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
如果想放弃此次合并的话，可以使用命令:
```
$ git merge --abort
```

当合并的分支衍生处在一条分支上时，Git会用Fast forward模式。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

使用--no-ff参数，可以禁用Fast forward：
```
$ git merge --no-ff -m "merge with no-ff" <branch_name>
```
用$ git log --graph命令可以看到分支合并图。
用带参数的git log也可以看到分支的合并情况：
```
$ git log --graph --oneline --abbrev-commit
```

# 版本操作
## 查看历史
* 查看所有分支的历史示意图:
```
$ git log --oneline --stat --decorate --graph --all -p
```

* 查看工作区与历史提交之间的差异
```
$ git diff
```

* 查看暂存区与历史提交之间的差异
```
$ git diff --cached
```

* 查看工作区与历史提交之间的差异
```
$ git diff <commit_id>
```

* 查看暂存区与历史提交之间的差异
```
$ git diff --cached <commit_id>
```

* 查看历史提交之间的差异
```
$ git diff <commit_id> <commit_id>
```

* 查看工作区与历史提交的某个文件的单词差异
```
$ git diff --color-words [--] <file>
$ git diff --word-diff [--] <file>
```

## 撤销操作
* 恢复暂存区某个文件的某个历史版本
```
$ git checkout <commit_id> [--] <file> 
```

* 恢复工作区某个文件的某个历史版本
```
$ git reset <commit_id> [--] <file> 
```
---
清除操作:
$ git clean 
-n 查看准备移除工作区文件
-x 移除忽略文件内的表达式的文件
-X 移除忽略文件内的表达式的文件及工作区文件
-f 执行移除工作区文件

---
移除某次提交的内容:
```
$ git revert <commit_id>
```

## 修改历史
替换最后一次提及
$ git commit --amend

将当前分支设置为另一分支的衍生分支
$ git rebase <branch_name>

要重返历史版本，可以用$ git reflog查看历史操作，以便确定要回到哪个版本。
版本回退:
```
HEAD指向的版本就是当前版本，Git允许我们在版本的历史之间穿梭，
~和^均可代表向上一个版本
$ git reset --hard HEAD~  # 回到上一个版本；
$ git reset --hard HEAD^^  # 回到上两个版本；
$ git reset --hard <commit_tag>  # 回退到指定版本。
```
--hard 使用某次提交的内容还原暂存区和工作区
--mixed 只使用某次提交的内容还原暂存区，并把HEAD和分支引用指向指定的commit
--soft 只把HEAD和分支引用指向指定的commit

# 远程仓库
## 创建SSH Key
生成ssh密钥
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
保存ssh密钥
```
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_rsa
```
登陆GitHub，打开“Account settings”，“SSH Keys”页面
点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
验证ssh密钥
```
$ ssh -T git@github.com
Hi xueyikang! You've successfully authenticated, but GitHub does not provide shell access.
```
# 远程协作
## 从远程库克隆
可以用命令git clone克隆一个本地库：
$ git clone <http_url | ssh_url>
或者建立本地仓库与远程仓库的联系
$ git remote add <origin | else_name> <http_url | ssh_url>

查看所有远程库的配置信息：
$ git remote -v

查看远程库的更详细的信息:
$ git remote shop <branch_name>

重命名远程仓库
$ git remote rename <origin_name> <else_name>

删除远程仓库
$ git remote rm <origin_name>

## 远程库协作
没有关联关系的话需要先要使用命令创建本地分支与远程origin的链接关系：
$ git checkout -b <local_branch_name> origin <remote_branch_name>

把本地库的所有内容推送到远程库上：
$ git push [-u] origin <branch_name>
我们第一次推送新分支时，加上了-u参数，Git不但会把本地分支内容推送的远程新分支，还会把本地分支和远程分支关联起来，在以后的推送或者拉取时就可以简化命令。

获取远程分支的最新代码到本地的远程分支上:
$ git fetch [origin [branch_name]

获取远程分支的最新代码到本地的远程分支上并与本地分支合并:
$ git pull [origin [branch_name]
如果提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，
用命令建立:$ git branch --set-upstream <local_branch_name> origin <remote_branch_name>。

如果是是两个不同的项目，则需要先 git pull --allow-unrelated-histories

删除远程分支
```
$ git push --delete origin <branch_name>
或者
$ git push origin :<branch_name>
```

# 搭建Git服务器
1. 安装git：
$ sudo apt-get install git

2. 创建一个git用户，用来运行git服务：
$ sudo adduser git

3. 创建证书登录：
收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

4. 初始化Git仓库：
先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
$ sudo git init --bare sample.git
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
$ sudo chown -R git:git sample.git

5. 禁用shell登录：
出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：
git:x:1001:1001:,,,:/home/git:/bin/bash
改为：
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

6. 克隆远程仓库：
现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：
$ git clone git@server:/srv/sample.gitCloning into 'sample'...warning: You appear to have cloned an empty repository.
剩下的推送就简单了。

# 管理公钥
如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。
这里我们不介绍怎么玩Gitosis了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。
管理权限
有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。
这里我们也不介绍Gitolite了，不要把有限的生命浪费到权限斗争中。
