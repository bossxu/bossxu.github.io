---
title: git重学笔记
date: 2019-06-13 23:57:57
tags: [git,自我成长]
---

### git笔记
[git文档](https://git-scm.com/book/zh/v2/)
> 之前用git，都是配合github一起使用来做一个像存代码得百度云，没有用到git的版本控制这一个主要的，感觉还是不太行，就把一些常用的自己来写个笔记吧，不然每次都去百度，也太惨了。

### git基础配置

一般在新的电脑上安装git，windows直接搞搞就行，但是linux要是想直接去apt-get下载可能会出现问题，有的没有库，还得自己去加一下库才行。然后下完第一件事就是配置一下git的用户。
```git
git config --global user.name "name"
git config --global user.email "example@email.com"
git config --list //配置展示
git config <key> //某项配置的属性
```
**global**是全局的意思

### 常用git命令

#### git add 
为哪些文件加入跟踪
```git
$ git add .
$ git add *.cpp
$ git commit -m 'message'
```

#### git status
看哪些文件在什么状态。
```git
$ git status
$ git status -s // 缩减版
```

#### git 忽略某些文件
就是进入**.gitignore**
+ 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
+ 可以使用标准的 glob 模式匹配
+ 匹配模式可以以（/）开头防止递归
+ 匹配模式可以以（/）结尾指定目录。
+ 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反
```.gitignore
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

#### git diff
用来看哪些不同，哪些改过啥得

#### git rm
对文件进行删除，还是要去移除这个跟踪文件才行，不过讲道理，好像也没有感觉到要用这个命令，还是说ide已经集成好了这些东西？

#### git log
看提交日志
```git
$ git log -p -2 // -p表示显示内容差异，-2表示显示最近两次的提交
```

#### git撤销操作

```git
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
这个只有一次的提交，如果之前提交的没有修改就能直接用，这样最后只会提交一次。

**取消暂存的文件**
```git
$ git reset HEAD hello.md // 表示把hello.md这个文件从暂存的区域中取消掉这个
```

**撤销对文件的修改**
文件修改就被撤销了
```git
$ git checkout -- test.md
```

#### git remote
这个就是git远程库的一些指令。

**git clone**
克隆远程仓库
```git
$ git clone <url>
$ git clone <url> 本地仓库名
```
url里面一般有两种，一种是https开头的，一种是git开头的，具体有啥利弊我也不是很清楚，主要应该还是和权限有关系吧。

**git remote**
```git
$ git remote // 显示远程服务器缩写
$ git remote -v // 显示远程服务器缩写和对应url
$ git remote add <shortname> <url> // 添加远程库
$ git fetch [remote_responsibity_name] // 从远端拉取数据，但是不会自动合并，需要手动去合并分支
$ git push [remote-name] [branch-name]
$ git remote show [remote-name] // 获得远程仓库的具体信息。
$ git remote rename [原名] [现在名]
$ git remote rm [仓库名]
```

**git tag**

给历史的某一个提交打标签，这个我没怎么碰到过 
标签分两种，轻量标签和附注标签
```git
$ git tag 
$ git tag -l 'v1.8*'
$ git tag -a [tagname] -m 'message' // 附注标签,git show 能看到
$ git push origin [tagname] // 共享标签
$ git push origin --tags //一次性推送很多标签
$ git tag -d [tagname] // 删除标签
```

### git 分支

> 这个是我想看的东西，之前和开源的写项目，还是对一些业务流程不是很了解，好像业务上面的一些操作都是重新开一个分支，再进行合并，不去影响主流的开发啥的

#### git branch

> 这个就相当于在某个提交对象上创建了一个指针，然后通过一个head的指针来指向当前所在的分支

```git
$ git branch [branch-name] // 创建一个新的分支
$ git log --oneline --decorate // 看这个分支指向的是哪个对象 
$ git checkout [branch-name] // 让head指针指向某个分支
$ git checkout -b [branch-name] //创建并且转向那个分支
$ git merge [branch-name] // 将当前的分支与后面的分支进行合并
$ git branch -d [branch-name] // 删除某个分支
```
#### git 远程分支

> 就是我用的代码版百度云的用法。
```git
$ git fetch [origin-name] // 抓取数据,但不会自动自动对
$ git push [remote] [branch]
```

#### git rebase

> 主流就两个一个是merge一个是rebase，各有用处吧，但是感觉rebase是为了一个线性的版本模式，rebase是把要并的分支之前的一个个版本都再次在这个分支进行加入。我觉得我讲不清楚，还是得看下原版得一些博客，说到这，不得不说感觉我自己得交流方面有点问题，虽然我听能扯的，但是

```git
$ git rebase [basebranch] [topicbranch]
```
