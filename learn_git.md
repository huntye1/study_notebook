# git指令学习

* `git commit` 提交一次记录
* `git branch  <name> ` 创建一个分支
* `git checkout  <name> `切换分支
* `checkout -b  <name>` 创建并切换分支
* `git merge <name>`把指定分支合并到该分支，生成一次记录。指定分支不变。
* `git rebase <name>` 把该分支移动到指定分支上，得到更加线性的提交序列。之前的提交记录依然存在。新的提交记录是之前的副本。或者`git rebase <name1> <name2>`把name2 移动到name1分支上。
* `git checkout <commit_name>` 切换head指向，branch name（如：master）总是指向该branch的尾巴。如果要查看其他提交记录，必须用该命令，改变head指向（一般head -> branch_name -> last_commit），此时进入分离HEAD状态（detached HEAD）。具体：https://git-scm.com/docs/git-checkout#_detached_head
* `git log ` 查看提交的记录详细信息（基于SHA-1共40位的哈希值），git中会智能处理，只需要输入前几个字符。
* `^`向前移动一个提交记录。 `~<num>` 向前移动多个提交记录。如`git  checkout master^` 。这就是哈希的另一种简便用法。
* `-f` 可以强制修改分支位置。例如：`git branch -f master HEAD~3` 将master分支强制指向HEAD的第三次夫提交。
* `git reset`  通过把分支记录回退几个提交记录来实现撤销改动。`git reset <hash>`。（被回退的变更还在，只是处于未加入暂存的状态。）用于本地撤销很方便，对于远程分支无效。
* `git revert` 撤销分支并分享给别人。例如`git revert HEAD`生成新的提交。该提交与HEAD指向的上一次提交相同。可以把更改推送到远程仓库。
* `git cherry-pick <hash1> <hash2> ...`把选中的提交记录 放在当前分支(HEAD)。
* `git rebase -i HEAD~4` 交互式rebase ，把当前分支到HEAD前四步的提交rebase。--interactive 。可以调整提交顺序。删除不想要的提交。合并提交。
* `git commit --amend` 对上次提交的内容或日志进行修改。不会生成新的提交记录。
* `git tag <tag_name> <hash>`  如果不指定提交`<hash>`，默认为HEAD。
* `git describe  <hash>` 得到离该提交最近的标签。 得到`<tag>_<numCommits>_g<hash>` 依次是tag名字、距离该提交的距离、hash。
* `^num `可以指定到哪一个父提交。
* `git checkout HEAD~^2~2`支持链式调用。

## 远程仓库

* `git clone` **本地**创建一个远程仓库的拷贝
* `orgin/master` 本地分支对应着远程仓库的分支。只有远程仓库的master更新，该分支才会更新。（checkout 并提交并不会影响origin/master的指向，此时HEAD进入分离模式），反应了本地与远程分支**最后一次通信**的状态。
* `git fetch `1.从远程仓库下载本地仓库中缺失的提交记录。2.更新远程分支指针（如 o/master)。实际上就是把**本地仓库**的远程分支更新成**远程仓库**对应分支最新的状态。使用的是`http://` 或`git://`协议。该操作只是把本地仓库与远程仓库同步了，并没有修改本地的文件，只是单纯的下载操作。
* `git pull`实际上就是合并的两个操作，先抓取（fetch），再合并(merge)。`git pull --rebase`是先抓取（fetch）再变基（rebase）。
* `git push`上传到远程仓库，并且合并提交。
* `git checkout -b totallyNotMaster o/master`关联本地分支到远程分支。或者：git branch -u o/master foo 如果就在foo分支，还可以省略foo。
* `git push <name> <place>`例如：`git push origin master`  *切到本地仓库中的“master”分支，获取所有的提交，再到远程仓库“origin”中找到“master”分支，将远程仓库中没有的提交记录都添加上去，搞定之后告诉我。*实际上就是**同步**。 如果直接`git push`，HEAD指向的分支推送给远程分支（如果HEAD指向没有分支，那么失败）。
* `git push origin <sorce>:<destination> `指定独立的来源和目的分支。
* `git fetch origin foo`更新对应不存在的提交到**origin/foo** 也可以`git fetch origin <sorce>:<destination> `但是不常用。
* `git push origin :<des>`空的`source`会删除远程的对应分支。
* `git fetch origin :<des>`空的`source`会新建本地的对应分支。
* `git pull origin master` 抓取远程的`master`到`o/master`分支到当前检出的分支。
* `git pull origin master:foo`抓取远程的`master`分支到`o/foo`分支，然后合并到当前检出分支。