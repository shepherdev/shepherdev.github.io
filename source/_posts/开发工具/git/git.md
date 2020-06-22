---
title: git指令
date: 2020-3-31
author: shepherd
toc: true
categories: [开发工具,git]
---

记录一些日常常用指令，参考
[廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/896043488029600/896827951938304)

> ssh协议不需要每次输密码，使用的是密钥对
>
> https协议需要每次输入账户名和密码

<!-- more -->

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

`git init`初始化git repo

`git add filename1 filename2`添加至暂存区

`git commit -m "commit-mes"`提交到本地库，会得到40位的哈希值id，类似快照

- 直接输入git commit会进入vim让你写提交信息
- 使用单引号输入多行信息

`git status`显示工作区状态

`git diff filename`如果有文件被修改过，可以使用diff查看修改的内容

- diff格式@@ -1,2 +1,4@@表示“修改前的1行开始连续2行变成了修改后的1行连续4行”

`git log`查看提交历史

`git reset --hard commit_id`回退版本

- HEAD^表示前一个版本
- HEAD3表示三个^

`git reflog`查看命令历史，可以得到每个版本的commit_id

`git checkout -- filename`丢弃工作区的修改回到最新版本

`git reset HEAD filename`该文件回到最近一次commit或者丢弃暂存区的修改

`git rm filename`删除文件，然后提交；手动删除，add-commit也行

### 连接远程仓库

在家目录下打开终端输入`ssh-keygen -t rsa -C email.com`

得到密钥对，将公钥添加到远程仓库即可

`git remote add origin git@sever-name:path/repo-name.git`关联远程仓库

`git push -u origin master`第一次推送

`git push orgin master`后续推送（可选择不同的分支）

`git clone git@sever-name:path/repo-name.git`克隆远程仓库

- 默认克隆只有master分支

- 想要在其他分支工作必须创建并关联远程分支

- `git switch -c branch-name origin/branch-name`创建，此时已经可以提交了

  - 当你和其他工作伙伴同时在同一分支提交同一个修改时，会出现以下情况

  ```bash
  $ git push origin dev 
  Username for 'https://github.com': yangchaohe
  Password for 'https://yangchaohe@github.com': 
  To https://github.com/yangchaohe/learngit.git
   ! [rejected]        dev -> dev (fetch first)
  error: 推送一些引用到 'https://github.com/yangchaohe/learngit.git' 失败
  提示：更新被拒绝，因为远程仓库包含您本地尚不存在的提交。这通常是因为另外
  提示：一个仓库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
  提示：（如 'git pull ...'）。
  提示：详见 'git push --help' 中的 'Note about fast-forwards' 小节。
  # shepherd @ MY in ~/learngit on git:dev o [20:26:31] C:1
  $ git pull 
  remote: Enumerating objects: 5, done.
  remote: Counting objects: 100% (5/5), done.
  remote: Compressing objects: 100% (2/2), done.
  remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
  展开对象中: 100% (3/3), 284 字节 | 94.00 KiB/s, 完成.
  来自 https://github.com/yangchaohe/learngit
     f3e318a..df2aa02  dev        -> origin/dev
  当前分支没有跟踪信息。
  请指定您要合并哪一个分支。
  详见 git-pull(1)。
  
      git pull <远程> <分支>
  
  如果您想要为此分支创建跟踪信息，您可以执行：
  
      git branch --set-upstream-to=origin/<分支> dev
  ```

  - 使用`git branch --set-upstream-to=origin/<分支> dev`关联远程仓库的分支
  - 再使用`git pull`将其他提交抓取下来进行修改再push
  - `git remote -v`查看远程仓库信息

- 多个伙伴在同一个分支提交时，提交历史会分叉，使用`git rebase`即可

### 分支

`git branch`查看当前分支

`git branch <name>`create branch

`git switch/checkout <name>`切换分支

`git switch -c <name>,git checkout -b <name>`创建并切换

`git merge <name> -m <message>`合并分支

`git branch -d <name>`删除分支

如果两个分支同时修改同一个文件的同一行内容，合并时就会发生冲突（修改不同行不会）

此时查看文件内容如下

```bash
 <<<<<<< HEAD
 master90
 =======
 test190
 >>>>>>> test1
```

需要修改再提交，然后使用`git log --graph --pretty=oneline --abbrev-commit`可以查看分支合并情况

```bash
*   f47d28d (HEAD -> master) confict fix
|\  
| * 9c16d81 (test1) test1
* | 082c980 master
* | 0ada1fe Merge branch 'test1'
|\| 
| * 3b4a2fd 位于分支 test1 要提交的变更：   （使用 "git restore --staged <文件>..." 以取消暂存）        修改：     readme.txt
* | 771f5e1 位于分支 master 要提交的变更：   （使用 "git restore --staged <文件>..." 以取消暂存）       修改：     readme.txt
|/  
* 73c59da tset
* 48be542 remove rademetxt
* bd16a3e A A A A
* bb669d2 test test2
```

使用`git log --graph`可以查看详细提交信息

> 注意，`git merge`默认是“Fast-forword（改变指针指向）”模式，这样的合并是没有记录历史分支的，从图上也就看不出是否分支，需要加上`--no-ff`使用普通模式合并
>
> 一般工作都是在dev上，master用来发布

- 当在其他的分支工作没完，想把工作状态存储在那个分支时，需要使用stash

`git stash`将工作状态暂存，可存储多个

`git stash pop`取出最后放入的工作状态，并删除stash的内容

`git stash apply@{index}`指定取出，不删除

`git stash drop`删除最后放入的工作状态

`git stash show`显示详细信息

> 存储顺序有点像先进后出

- 当在别的分支修改的数据想应用到其他分支时，只需要在其他分支复制一份提交即可

指令：`git cherry-pick <commit_id>`

复制除了提交id不一样，其他包括提交信息，时间都一样

- 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D `强行删除

### 标签

描述每个版本不可能用哈希值，git使用tag与commit_id结合起来

`git tag <tagname> <commit_id>`会默认把当前的HEAD打上标签，可在后面指定commit-id

`git tag <tagname> -m "描述"`

`git tag`查看所有标签

`git show <tagname>`显示详细信息

`git push origin <tagname>`推送某标签到远程仓库

`git push origin --tags`推送所有标签

 `git tag -d <tagname>`删除本地标签

删除远程仓库的标签要先删本地标签再使用指令

`git push origin :refs/tags/<tagname>`

### 忽略

如果需要忽略某些文件时，可以编写.gitgnore文件，在里面写上文件名即可，[官方模板](https://github.com/github/gitignore)

### 别名

```bash
git config --global alias.st status
git config --global alias.unstage "reset HEAD"
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.co checkout
git config --global alias.st status
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### status中文乱码

默认的配置里, 使用中文命名并使用git status查看会得到中文的字符码, 可以通过下面的指令解决

```bash
git config --global core.quotepath false
```

或者修改配置文件`~/.gitconfig`

```bash
[gui]  
    encoding = utf-8  
    # 代码库统一使用utf-8  
[i18n]  
    commitencoding = utf-8  
    # log编码  
[svn]  
    pathnameencoding = utf-8  
    # 支持中文路径  
[core]
    quotepath = false 
    # status引用路径不再是八进制（反过来说就是允许显示中文了）
```

> 参考[解决git-status不能显示中文](https://blog.csdn.net/u012145252/article/details/81775362#解决git-status不能显示中文)