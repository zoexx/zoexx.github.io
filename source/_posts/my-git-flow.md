title: 使用git flow
date: 2018-06-20 17:54:29
tags: git git-flow
---

- 这里有一份中文的简单说明 👉 [git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
- 命令行说明 👉 [Command-Line-Arguments](https://github.com/nvie/gitflow/wiki/Command-Line-Arguments)

```bash
λ git flow init
Initialized empty Git repository in C:/Users/ibesteeth/Documents/myGitFlow/.git/
No branches exist yet. Base branches must be created now.
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Bugfix branches? [bugfix/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []
Hooks and filters directory? [C:/Users/ibesteeth/Documents/myGitFlow/.git/hooks]
```

| name | discription |
| ---- | ----------- |
|Feature| 特性，功能开发 feature/a_new_feature |
|Bugfix | 修复bug bugfix/fix_a_button      |
|Release| 支持一个新的用于生产环境的发布版本。允许修正小问题，并为发布版本准备元数据。 release/v1.0.0     |
|Hotfix | 紧急修复一个已经release的版本 hotfix/v1.0.1 |
|Support| The 'support' feature is still beta, using it is not advised |
|Version tag prefix | 版本号前缀 默认为空 可以使用 'v' 0.0.0 => v0.0.0 |
|Hooks and filters| git 的钩子设置 |


```bash
λ git flow help
usage: git flow <subcommand>

Available subcommands are:
   init      Initialize a new git repo with support for the branching model.
   feature   Manage your feature branches.
   bugfix    Manage your bugfix branches.
   release   Manage your release branches.
   hotfix    Manage your hotfix branches.
   support   Manage your support branches.
   version   Shows version information.
   config    Manage your git-flow configuration.
   log       Show log deviating from base branch.

Try 'git flow <subcommand> help' for details.
```

git flow 的相关配置项可以在 .git/config 中找到

#### 关于git hooks

和其它版本控制系统一样，Git 能在特定的重要动作发生时触发自定义脚本。在项目的 .git/hooks/ 下可以找到所有可以使用的钩子。

除了 **pre-receive** **update** **post-receive** 属于服务端使用的钩子， 其他均为客户端钩子。

> 需要注意的是，克隆某个版本库时，它的客户端钩子 **并不** 随同复制。

- applypatch-msg.sample 电邮工作流 由 git am 命令开始时触发

```bash
#!/bin/sh
#
# An example hook script to check the commit log message taken by
# applypatch from an e-mail message.
#
# The hook should exit with non-zero status after issuing an
# appropriate message if it wants to stop the commit.  The hook is
# allowed to edit the commit message file.
#
# To enable this hook, rename this file to "applypatch-msg".

. git-sh-setup
commitmsg="$(git rev-parse --git-path hooks/commit-msg)"
test -x "$commitmsg" && exec "$commitmsg" ${1+"$@"}
:
```

- pre-applypatch.sample 电邮工作流 由 git am 运行期间触发，它正好运行于应用补丁 之后，产生提交之前

```bash
#!/bin/sh
#
# An example hook script to verify what is about to be committed
# by applypatch from an e-mail message.
#
# The hook should exit with non-zero status after issuing an
# appropriate message if it wants to stop the commit.
#
# To enable this hook, rename this file to "pre-applypatch".

. git-sh-setup
precommit="$(git rev-parse --git-path hooks/pre-commit)"
test -x "$precommit" && exec "$precommit" ${1+"$@"}
:
```

- commit-msg.sample

```bash
#!/bin/sh
#
# An example hook script to check the commit log message.
# Called by "git commit" with one argument, the name of the file
# that has the commit message.  The hook should exit with non-zero
# status after issuing an appropriate message if it wants to stop the
# commit.  The hook is allowed to edit the commit message file.
#
# To enable this hook, rename this file to "commit-msg".

# Uncomment the below to add a Signed-off-by line to the message.
# Doing this in a hook is a bad idea in general, but the prepare-commit-msg
# hook is more suited to it.
#
# SOB=$(git var GIT_AUTHOR_IDENT | sed -n 's/^\(.*>\).*$/Signed-off-by: \1/p')
# grep -qs "^$SOB" "$1" || echo "$SOB" >> "$1"

# This example catches duplicate Signed-off-by lines.

test "" = "$(grep '^Signed-off-by: ' "$1" |
         sort | uniq -c | sed -e '/^[   ]*1[    ]/d')" || {
        echo >&2 Duplicate Signed-off-by lines.
        exit 1
}
```

- pre-commit.sample

``` bash
# An example hook script to verify what is about to be committed.
# Called by "git commit" with no arguments.  The hook should
# exit with non-zero status after issuing an appropriate message if
# it wants to stop the commit.
```

- prepare-commit-msg.sample

```bash
# An example hook script to prepare the commit log message.
# Called by "git commit" with the name of the file that has the
# commit message, followed by the description of the commit
# message's source.  The hook's purpose is to edit the commit
# message file.  If the hook fails with a non-zero status,
# the commit is aborted.
#
# To enable this hook, rename this file to "prepare-commit-msg".

# This hook includes three examples. The first one removes the
# "# Please enter the commit message..." help message.
#
# The second includes the output of "git diff --name-status -r"
# into the message, just before the "git status" output.  It is
# commented because it doesn't cope with --amend or with squashed
# commits.
#
# The third example adds a Signed-off-by line to the message, that can
# still be edited.  This is rarely a good idea.
```

- fsmonitor-watchman.sample

```bash
# An example hook script to integrate Watchman
# (https://facebook.github.io/watchman/) with git to speed up detecting
# new and modified files.
#
# The hook is passed a version (currently 1) and a time in nanoseconds
# formatted as a string and outputs to stdout all files that have been
# modified since the given time. Paths must be relative to the root of
# the working tree and separated by a single NUL.
#
# To enable this hook, rename this file to "query-watchman" and set
# 'git config core.fsmonitor .git/hooks/query-watchman'
```

- post-update.sample

```bash
#!/bin/sh
#
# An example hook script to prepare a packed repository for use over
# dumb transports.
#
# To enable this hook, rename this file to "post-update".

exec git update-server-info
```


- pre-push.sample

```bash
# An example hook script to verify what is about to be pushed.  Called by "git
# push" after it has checked the remote status, but before anything has been
# pushed.  If this script exits with a non-zero status nothing will be pushed.
#
# This hook is called with the following parameters:
#
# $1 -- Name of the remote to which the push is being done
# $2 -- URL to which the push is being done
#
# If pushing without using a named remote those arguments will be equal.
#
# Information about the commits which are being pushed is supplied as lines to
# the standard input in the form:
#
#   <local ref> <local sha1> <remote ref> <remote sha1>
#
# This sample shows how to prevent push of commits where the log message starts
# with "WIP" (work in progress).
```

- pre-rebase.sample

```bash
# The "pre-rebase" hook is run just before "git rebase" starts doing
# its job, and can prevent the command from running by exiting with
# non-zero status.
#
# The hook is called with the following parameters:
#
# $1 -- the upstream the series was forked from.
# $2 -- the branch being rebased (or empty when rebasing the current branch).
#
# This sample shows how to prevent topic branches that are already
# merged to 'next' branch from getting rebased, because allowing it
# would result in rebasing already published history.
```

- pre-receive.sample

```bash
# An example hook script to make use of push options.
# The example simply echoes all push options that start with 'echoback='
# and rejects all pushes when the "reject" push option is used.
```

- update.sample

```bash
# An example hook script to block unannotated tags from entering.               
# Called by "git receive-pack" with arguments: refname sha1-old sha1-new        
#                                                                               
# To enable this hook, rename this file to "update".                            
#                                                                               
# Config                                                                        
# ------                                                                        
# hooks.allowunannotated                                                        
#   This boolean sets whether unannotated tags will be allowed into the         
#   repository.  By default they won't be.                                      
# hooks.allowdeletetag                                                          
#   This boolean sets whether deleting tags will be allowed in the              
#   repository.  By default they won't be.                                      
# hooks.allowmodifytag                                                          
#   This boolean sets whether a tag may be modified after creation. By default  
#   it won't be.                                                                
# hooks.allowdeletebranch                                                       
#   This boolean sets whether deleting branches will be allowed in the          
#   repository.  By default they won't be.                                      
# hooks.denycreatebranch                                                        
#   This boolean sets whether remotely creating branches will be denied         
#   in the repository.  By default this is allowed.                             
```


#### 参考文档

[git book](https://git-scm.com/book/en/v2)