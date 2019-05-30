---
title: Git常用命令
date: 2017-05-13 17:49:35
tags:
  - git
cover: /img/cover_git_shell.png
---

- Workspace：工作区
- Index／Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

一、新建代码库

```bash
  # 在当前目录新建一个 Git 代码库
  $ git init

  # 新建一个目录，将其初始化为 Git 代码库
  $ git init [project-name]

  # 下载一个项目和它的整个代码历史
  $ git clone [url] [local-path]
```

二、配置

Git 的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```bash
  # 显示当前的 Git 配置
  $ git config --list

  # 编辑 Git 配置文件
  $ git config -e [--global]

  # 设置提交代码时的用户信息
  $ git config [--global] [user.name](http://user.name) "[name]"
  $ git config [--global] user.email "[email address]"

  # 生成密钥
  $ ssh-keygen -t rsa -C "[xxx@gmail.com]"

  # 查看密钥
  $ cat ~/.ssh/id_rsa.pub

  # 测试密钥
  $ ssh -T git@github.com
```

三、增加/删除文件

```bash
  # 添加指定文件到暂存区
  $ git add [file1] [file2] ...

  # 添加指定目录到暂存区，包括子目录
  $ git add [dir]

  # 添加所有改变的文件
  $ git add -A .

  # 添加所有内容
  $ git add -A

  # 添加当前目录的所有文件到暂存区
  $ git add .

  # 添加编辑或者删除的文件，不包括新添加的文件
  $ git add -u

  # 删除工作区文件，并且将这次删除放入暂存区
  $ git rm [file1] [file2] ...

  # 停止追踪指定文件，但该文件会保留在工作区
  $ git rm --cached [file]

  # 改名文件，并且将这个改名放入暂存区
  $ git mv [file-original] [file-renamed]
```

四、代码提交

```bash
  # 提交暂存区到仓库区
  $ git commit -m [message]

  # 提交暂存区的指定文件到仓库区
  $ git commit [file1] [file2] ... -m [message]

  # 提交工作区自上次 commit 之后的变化，直接到仓库区
  $ git commit -a

  # 提交时显示所有 diff 信息
  $ git commit -v

  # 使用一次新的 commit，替代上一次提交，如果代码没有任何新变化，则用来改写上一次 commit 的提交信息
  $ git commit --amend -m [message]

  # 重做上一次 commit，并包括指定文件的新变化
  $ git commit --amend ...
```

五、分支

```bash
  # 列出所有本地分支
  $ git branch

  # 列出所有远程分支
  $ git branch -r

  # 列出所有本地分支和远程分支
  $ git branch -a

  # 查看每个分支的最新提交记录
  $ git branch -av

  # 列出本地分支和远程分支关系
  $ git branch -vv

  # 新建一个分支，但依然停留在当前分支
  $ git branch [branch-name]

  # 新建一个分支，并切换到该分支
  $ git checkout -b [branch]

  # 新建一个分支，指向指定 commit
  $ git branch [branch] [commit]

  # 新建一个分支，与指定的远程分支建立追踪关系
  $ git branch --track [branch] [remote-branch]

  # 从远程检出一个分支
  $ git checkout -b [branch] [origin/branch]

  # 切换到指定分支，并更新工作区
  $ git checkout [branch-name]

  # 建立追踪关系，在现有分支与指定的远程分支之间
  $ git branch --set-upstream [branch] [remote-branch]

  # 合并指定分支到当前分支
  $ git merge [branch]

  # 选择一个 commit，合并进当前分支
  $ git cherry-pick [commit]

  # 删除分支
  $ git branch -d [branch-name]

  # 强制删除分支
   $ git branch -D [branch-name]

  # 删除远程分支
  $ git push origin --delete
  $ git branch -dr
```

六、标签

```bash
  # 列出所有 tag
  $ git tag

  # 新建一个 tag 在当前 commit
  $ git tag [tag]

  # 新建一个 tag 在指定 commit
  $ git tag [tag] [commit]

  # 查看 tag 信息
  $ git show [tag]

  # 提交指定 tag
  $ git push [remote] [tag]

  # 提交所有 tag
  $ git push [remote] --tags

  # 新建一个分支，指向某个 tag
  $ git checkout -b [branch] [tag]

  # 删除本地所有tag
  $ git tag -l | xargs git tag -d

  # 下载远程所有tag
  $ git fetch -t
```

七、查看信息

```bash
  # 显示有变更的文件
  $ git status

  # 显示当前分支的版本历史
  $ git log

  # 显示 commit 历史，以及每次 commit 发生变更的文件
  $ git log --stat

  # 显示某个文件的版本历史，包括文件改名
  $ git log --follow [file]
  $ git whatchanged [file]

  # 显示指定文件相关的每一次 diff
  $ git log -p [file]

  # 显示指定文件是什么人在什么时间修改过
  $ git blame [file]

  # 显示暂存区和工作区的差异
  $ git diff

  # 显示暂存区和上一个 commit 的差异
  $ git diff --cached []

  # 显示工作区与当前分支最新 commit 之间的差异
  $ git diff HEAD

  # 显示两次提交之间的差异
  $ git diff [first-branch]...[second-branch]

  # 显示某次提交的元数据和内容变化
  $ git show [commit]

  # 显示某次提交发生变化的文件
  $ git show --name-only [commit]

  # 显示某次提交时，某个文件的内容
  $ git show [commit]:[filename]

  # 显示当前分支的最近几次提交
  $ git reflog
```

八、远程同步

```bash
  # 下载远程仓库的所有变动
  $ git fetch [remote]

  # 显示所有远程仓库
  $ git remote -v

  # 显示某个远程仓库的信息
  $ git remote show [remote]

  # 查看远程分支与本地分支对应关系
  $ git remote show origin

  # 删除远程已经不存在的分支
  $ git remote prune origin

  # 删除跟踪仓库地址
  $ git remote rm origin

  # 增加一个新的远程仓库，并命名
  $ git remote add origin [url]

  # 修改远程仓库地址
  $ git remote set-url origin [url]

  # 取回远程仓库的变化，并与本地分支合并
  $ git pull [remote] [branch]

  # 上传本地指定分支到远程仓库
  $ git push [remote] [branch]

  # 提交到服务器并在服务器新建dev分支
  $ git push --set-upstream origin dev

  # 强行推送当前分支到远程仓库，即使有冲突
  $ git push [remote] --force

  # 推送所有分支到远程仓库
  $ git push [remote] --all
```

九、撤销

```bash
  # 恢复暂存区的指定文件到工作区
  $ git checkout [file]

  # 恢复某个 commit 的指定文件到工作区
  $ git checkout [commit] [file]

  # 恢复上一个 commit 的所有文件到工作区
  $ git checkout .

  # 重置暂存区的指定文件，与上一次 commit 保持一致，但工作区不变
  $ git reset [file]

  # 重置暂存区与工作区，与上一次 commit 保持一致
  $ git reset --hard

  # 重置当前分支的指针为指定 commit，同时重置暂存区，但工作区不变
  $ git reset [commit]

  # 重置当前分支的 HEAD 为指定 commit，同时重置暂存区和工作区，与指定 commit 一致
  $ git reset --hard [commit]

  # 重置当前 HEAD 为指定 commit，但保持暂存区和工作区不变
  $ git reset --keep [commit]

  # 新建一个 commit，用来撤销指定 commit，后者的所有变化都将被前者抵消，并且应用到当前分支
  $ git revert [commit]
```

十、其他

```bash
  # 生成一个可供发布的压缩包
  $ git archive
```

十一、常见问题

1、远程分支获取最新的版本到本地

- 执行git pull命令
- 如果以上命令还是失败尝试以下步骤：
  1. 首先从远程的origin的master主分支下载最新的版本到origin/master分支上<br>`git fetch origin master`

  2. 比较本地的master分支和origin/master分支的差别<br>`git log -p master..origin/master`

  3. 进行合并<br>`git merge origin/master`

2、如何解决`failed to push some refs to git`

  ```bash
  # 进行代码合并
  $ git pull --rebase origin master

  # 完成代码上传
  $ git push -u origin master
  ```

3、如何解决`If you wish to set tracking information for this branch you can do so with: git branch --set-upstream-to=origin/ master`

- 指定当前当前工作目录工作分支，跟远程仓库分支之间的关系
  `git branch --set-upstream master origin/master`

4、`git pull`获取最新代码报以下错误 `fatal: refusing to merge unrelated histories`

- git pull之后加上可选参数 --allow-unrelated-histories 强制合并
  `git pull origin master --allow-unrelated-histories`

5、`.gitignore`规则不生效的解决办法

- 把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被追踪的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未被追踪状态），然后再提交：
  ```bash
  git rm -r --cached . 或者 git rm -r README.md
  git add .
  git commit -m 'update .gitignore'
  ```