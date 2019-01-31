title: git指北
date: 2018-07-04 15:53:07
tags: git
---

> 整理自7月在公司的分享

# ❤Git

## why git

##### 查看修改历史
![github desktop](http://cdn.zoeservers.com/md-art/01.png)

##### 查看分支情况
![git gui](http://cdn.zoeservers.com/md-art/02.png)

##### 查看谁改了哪一行
![git blame](http://cdn.zoeservers.com/md-art/03.png)

##### 分布式版本控制
![git server](http://cdn.zoeservers.com/md-art/04.png)

- 分布式版本控制系统
- 以commit为单位恢复到某一个版本
- 基于每一个文件每一行的跟踪
- 分支，多人协作
- 超快的速度和超大的数据量

## 理解git中文件的状态标识

![status](http://cdn.zoeservers.com/md-art/06.png)

## git的"五"个区

![status](http://cdn.zoeservers.com/md-art/08.png)

查看五个区的扭转关系👉[http://ndpsoftware.com/git-cheatsheet.html#loc=remote_repo;](http://ndpsoftware.com/git-cheatsheet.html#loc=remote_repo;)

## 使用git

- git的命令行指令
```bash
λ git --help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```

- 常用指令的补充

```bash
# 查看commit的变更
$ git log -p
# 查看变更行等数据统计
$ git log --stat
# 自定义输出样式 graph是带分支走势图的
$ git log --pretty=format:"%h %s" --graph
# 补充一个修改到上一个commit
$ git commit --amend
# 撤销CONTRIBUTING.md的上一次commit
$ git reset HEAD CONTRIBUTING.md
# 撤销对CONTRIBUTING.md的修改
$ git checkout -- CONTRIBUTING.md
# 新建一个分支
$ git checkout -b hotfix
# 合并一个分支
$ git merge hotfix
# 查看所有包含未合并工作的分支
$ git branch --no-merged
# 推送一个分支到远程仓库
$ git push [remote-name] [branch-name]
# 拉取远程仓库中所有分支的最新commit
$ git pull --all
# 并不像提交一个commit 但是需要切换分支时 可以将工作区的修改储藏起来
$ git stash
$ git stash pop
```


## 小技巧

- 修改环境变量来跟踪git的运行
```bash
$ GIT_TRACE=true git lga
20:12:49.877982 git.c:554               trace: exec: 'git-lga'
20:12:49.878369 run-command.c:341       trace: run_command: 'git-lga'
20:12:49.879529 git.c:282               trace: alias expansion: lga => 'log' '--graph' '--pretty=oneline' '--abbrev-commit' '--decorate' '--all'
20:12:49.879885 git.c:349               trace: built-in: git 'log' '--graph' '--pretty=oneline' '--abbrev-commit' '--decorate' '--all'
20:12:49.899217 run-command.c:341       trace: run_command: 'less'
20:12:49.899675 run-command.c:192       trace: exec: 'less'
```

- 使用别名
```bash
$ git config --global alias.st status
$ git st
```

- 使用git超强的统计
```bash
# 统计项目中commit数量前十的人
$ git log --pretty=%aN | sort | uniq -c | sort -k1 -n -r | head -n 10
# 统计有多少个代码贡献者
$ git log --pretty=%aN | sort -u | wc -l
# 统计某人的commit数量
$ git log --author="$(git config --get user.name)" --oneline | wc -l
# 统计某人的代码行数
git log --author="$(git config --get user.name)" --pretty=tformat: --numstat | awk '{adds += $1; subs += $2; all += $1 + $2} END {printf "added lines: %s removed lines : %s all lines: %s\n",adds,subs,all}'
added lines: 2797109 removed lines : 1886733 all lines: 4683842
```

## 常见报错处理

- hint: Updates were rejected because the remote contains work that you do
- The following untracked working tree files would be overwritten by merge
- Your local changes to the following files would be overwritten by merge
- Your branch is ahead of ‘origin/master’ by 2 commits
- fatal: Unable to create '/path/my_proj/.git/index.lock': File exists.

## 理解git工作原理的关键字

- 内容寻址（content-addressable）文件系统
- 键值对数据库
- SHA-1 文件内容校验运算
- zlib压缩
- blob object , tree object , commit object , tag object
- 快照

```bash
$ echo 'test content' | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4

$ find .git/objects -type f
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4

$ git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4
test content
```

![tree object](http://cdn.zoeservers.com/md-art/05.png)

```bash
$ echo 'third commit'  | git commit-tree 3c4e9c -p cac0cab
1a410efbd13591db07496601ebc7a059dd55cfe9
$ git log
commit 1a410efbd13591db07496601ebc7a059dd55cfe9
Author: Scott Chacon <schacon@gmail.com>
Date:   Fri May 22 18:15:24 2009 -0700

	third commit
```

- .git 目录

```bash
$ ls -F1
HEAD            ---symbolic reference 指向目前所在的分支
index           ---暂存区信息
config*         ---配置选项
description     ---仅供 GitWeb 程序使用
hooks/          ---事件钩子，可以自定义脚本
info/           ---global exclude,not in .gitignore
objects/        ---储存数据内容
refs/           ---references 分支指向的commit object的key和remote reference
```

- git add = 将被改写的文件保存为blob object，更新暂存区，记录tree object
- git commit = 创建一个指明了顶层tree object和父commit object
- git gc 打包，存储差异
```bash
$ git gc
Counting objects: 18, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (14/14), done.
Writing objects: 100% (18/18), done.
Total 18 (delta 3), reused 0 (delta 0)
```

## 分支管理策略 git flow

> 以单个功能点(feature)为最小单位做分支

| 主分支名称 | 含义 |
| --------- | ---- |
| master    | 稳定线上版本分支 |
| develop   | 稳定的开发分支，所有的新功能基于develop开始开发 |
| release   | 上线版本 |
| hotfix    | 紧急修复，修改线上问题，基于master分支，改完后merge到master和develop |


## code review

> 为提高代码质量，一般会使用code review机制

什么时候触发一次review呢？

##### pull request （PR）

当你提交一个 Pull Request 的时候，你做的事情是 请求（request） 另一个开发者（比如项目维护者）来 拉取（pull） 你仓库中的一个分支到他们的仓库。也就是说你需要提供 4 个信息来完成一个 Pull Request：**源仓库**、**源分支**、**目标仓库**、**目标分支**。

##### git flow 与 PR 在 gitlab 中的运用
1. settings>repository>protected branches 中设置需要权限才可以合并的分支 Master/Develop 等等根据实际情况使用
2. 每次一个feature或者hotfix开发完毕需要合并分支到M/D时 在gitlab中发起
3. 由reviewer检查修改点，如有问题可以在这个请求下评论，由发起人解决问题，没有问题之后合并分支
4. 删除已合并的分支

![new merge request](http://cdn.zoeservers.com/md-art/09.png)


##### 合并分支的操作流程

例如将 featureA 合并到 develop
```bash
# current featureA
# 拉取一下所有分支的修改
git pull --all
# 将develop的最新修改拉到featureA检查一下有没有冲突，其实也可以直接在develop上合并featureA，这一步最好是pr提出方来做会提高效率，功能分支的改动需要关心主分支的进展
git merge develop
# 解决冲突
...
# 切换到develop合并已解决了冲突的featureA
git checkout develop
git merge featureA
# 提交
git push origin develop
# 删除已合并的分支 保持clean
# local
git branch -d featureA
# remote
git push origin :featureA
```

## 项目中使用gitlab的注意事项

- 创建仓库，需要注意权限问题，最好是在group中创建一个project
- gitignore
- 编写根目录的readme请指明部署相关的要点

```text
为了方便运维查看的项目readme.md 需要提及的部分

### 1. 项目介绍

### 2. 项目地址

- 线上地址
	- 模块一入口
	- 模块二入口
	- 模块三入口

- 测试环境地址

### 3. 部署说明

环境A => 分支A/xx
环境B => 分支B/xx

- 构建代码

---```bash
npm i 
npm run build
---```

- 目录结构（准确描述需要放在服务器上的目录）
```

## 资源

- [中文版git book](https://git-scm.com/book/zh/v2)
- [超级简单好用的git可视化桌面程序 github desktop](https://desktop.github.com/)
- [git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
