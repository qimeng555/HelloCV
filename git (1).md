" 版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统"

这个是严谨一点的说法

而通俗一点来讲就是，用来帮你记录这个文件的不同状态

就是说刚开始是怎么样

改动一次后是怎么样

改动两次后是怎么样

可以进行回溯查询的功能

工作区的大白话是git所在的目录下，除了.git之外的其他文件都是在工作区内

暂存区是用add命令放进文件的位置

暂存区相当于个人的库，用于自己文件的改动及保存

仓库就是被git管理的一个文件夹，追踪所有历史变化，可支持多人同时操作等一系列优点



git add .

将当前目录下所有文件添加到暂存区

$ touch README                # 创建文件

$ touch hello.php             # 创建文件

$ ls

README        hello.php

$ git status -s

?? README

?? hello.php

$ git add README hello.php 

添加后??变成A

如若改动，则变成AM

再次add变回A

及保存历史文件



git status 命令会显示以下信息：

当前分支的名称。

当前分支与远程分支的关系（例如，是否是最新的）。

未暂存的修改：显示已修改但尚未使用 git add 添加到暂存区的文件列表。

未跟踪的文件：显示尚未纳入版本控制的新文件列表

例如

$ git status

On branch master



Initial commit



Changes to be committed:

  (use "git rm --cached <file>..." to unstage)



    new file:   README

    new file:   hello.php



git diff 命令比较文件的不同，即比较文件在暂存区和工作区的差异



git pull 命令用于从远程获取代码并合并本地的版本

先从远程仓库获取最新的提交记录，然后将这些提交记录合并到你当前的分支中

git pull [远程仓库名] [分支名]

而与之相对的是

git push <远程主机名> <本地分支名>:<远程分支名>

用于从将本地的分支版本上传到远程并合并



git clone 命令可以复制远程仓库的所有代码和历史记录，并在本地创建一个与远程仓库相同的仓库副本





git checkout <branch-name>用于切换分支

例如git checkout master 切到主分支



git branch可用于查看分支和创建分支和删除分支

gir merge则用于合并分支



git fetch和git pull都用于更新代码，通过获取远程仓库的最新变更实现与本地的同步

使用git fetch更新代码后，本地的master分支的commitID保持不变需要用mergr来合并

而git pull则是结合了(fetch)和(merge)二者



git remote add <name> <url>用于添加远程仓库

git remote rename <old> <new>‌‌重命名

git remote show <name>详细信息

git remote remove <name>移出

等等命令



而撤销提交有git reset --soft HEAD~ 

恢复到历史版本git reset <commit> -- <file>

相似的git revert是用来反向补丁用的

若提交 A 引入了错误，用git revert A 会生成一个新的 B，其内容为 A 的反向操作，而 A 本身仍保留在历史中

最后

‌《git stash 是 Git 中用于临时保存工作目录和暂存区修改的命令，允许用户将未提交的更改存储到栈中，并恢复工作区到干净状态，便于切换分支或处理其他任务而不丢失进度》

这个是官方定义

我理解的是如若写文件写了一半发现途径错了

可用这个指令暂时保存

然后前往正确的路径打开继续编写



rebase工作流是用于管理代码提交历史和分支合并的方法

比如建立新的分支以后，可能在原来的分支上有了新的改变从而使得整块的"基地"发生了改变。

而使用rebase可以将文件列为一条简洁的历史线，直接合并分支



pull request的流程

1.配置远程仓库，建立特性分支

2.进行开发工作，然后提交修改，同步主分支最新代码，推送到远程仓库

3.创建pr

4.等待检查，改冲突

5.合并pr，清理分支



恢复误删信息

git reflog show --all

# 1. 找到要恢复的提交哈希（比如 d4e5f6a）

git reflog



# 2. 基于该提交创建新分支（安全做法）

git branch recovered-branch d4e5f6a



# 3. 切换到该分支检查内容

git checkout recovered-branch



# 4. 如果确认正确，可以合并回原分支

git checkout main

git merge recovered-branch



# 或者直接重置到该提交（危险，会丢失后续提交）

git reset --hard d4e5f6a



这个就是reflog的回复







