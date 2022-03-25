# Git-command[参考](https://mp.weixin.qq.com/s/PXu2YcNXvaRVxliKvnnYQg)
- git的一些常用命令
- 6步git提交到你的仓库
```
# 1.cd到你的文件夹
cd /yourfilename

#2.git 初始化
git init

#3.git 到本地
git add.

#4.git 远程链接
git remote add origin https://github.com/yourhub.git

#5.配置邮箱
 git config --global “youremail@yeah.net”
#6.push它进入你的main分支
git push -u origin master
#这个master可能得改一下 改成main 因为默认main 了不默认master了
```

## 1.我刚才提交了什么?
如果你用 git commit -a 提交了一次变化(changes)，而你又不确定到底这次提交了哪些内容。你就可以用下面的命令显示当前HEAD上的最近一次的提交(commit):
```
git show
```
或者
```
git log -n1 -p
```
## 2.我的提交信息(commit message)写错了

如果你的提交信息(commit message)写错了且这次提交(commit)还没有推(push), 你可以通过下面的方法来修改提交信息(commit message):
```
git commit --amend --only
```
这会打开你的默认编辑器, 在这里你可以编辑信息. 另一方面, 你也可以用一条命令一次完成:
```
git commit --amennd --only -m 'xxxxxxx'
```
如果你已经推(push)了这次提交(commit), 你可以修改这次提交(commit)然后强推(force push), 但是不推荐这么做。

## 3.我提交(commit)里的用户名和邮箱不对
如果这只是单个提交(commit)，修改它：
```
git commit --amend --author "new authorname <authorname@xx.com>"
```
如果你需要修改所有历史, 参考 **git filter-branch**的指南页.

## 4.我想从一个提交(commit)里移除一个文件
通过下面的方法，从一个提交(commit)里移除一个文件:
```
git checkout HEAD^ myfile
git add -A
git commit --amend
```
将非常有用，当你有一个开放的补丁(open patch)，你往上面提交了一个不必要的文件，你需要强推(force push)去更新这个远程补丁。
## 4.我想删除我的的最后一次提交(commit)

如果你需要删除推了的提交(pushed commits)，你可以使用下面的方法。可是，这会不可逆的改变你的历史，也会搞乱那些已经从该仓库拉取(pulled)了的人的历史。简而言之，如果你不是很确定，千万不要这么做。
```
git reset HEAD^ --hard
git push -f [remote] [bbranch]
```
如果你还没有推到远程, 把**Git重置reset**到你最后一次提交前的状态就可以了(同时保存暂存的变化):
```
git reset --soft HEAD@{1}
```
这只能在没有推送之前有用. 如果你已经推了, 唯一安全能做的是 git revert SHAofBadCommit， 那会创建一个新的提交(commit)用于撤消前一个提交的所有变化(changes)；或者, 如果你推的这个分支是rebase-safe的 (例如：其它开发者不会从这个分支拉), 只需要使用 git push -f。

## 5.删除任意提交(commit)

同样的警告：不到万不得已的时候不要这么做.
```
git rebase --onto SHA1_OF_BAD_COMMIT^ SHA1_OF_COMMIT
git push -f[remote] [brach]
```
## 6.我尝试推一个修正后的提交(amended commit)到远程，但是报错：
```
To https://github.com/yourusername/repo.git
! [rejected]        mybranch -> mybranch (non-fast-forward)
error: failed to push some refs to 'https://github.com/tanay1337/webmaker.org.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
注意, **rebasing(见下面)和修正amending**会用一个新的提交(commit)代替旧的, 所以如果之前你已经往远程仓库上推过一次修正前的提交(commit)，那你现在就必须强推(force push) (-f)。注意 – 总是 确保你指明一个分支!
```
git push origin mybrach -f
```
一般来说, 要避免强推. 最好是创建和推(push)一个新的提交(commit)，而不是强推一个修正后的提交。后者会使那些与该分支或该分支的子分支工作的开发者，在源历史中产生冲突。

## 7.我意外的做了一次硬重置(hard reset)，我想找回我的内容

如果你意外的做了 **git reset --hard**, 你通常能找回你的提交(commit), 因为Git对每件事都会有日志，且都会保存几天。
```
git reflog
```
你将会看到一个你过去提交(commit)的列表, 和一个重置的提交。选择你想要回到的提交(commit)的SHA，再重置一次:
```
git reset --hard SHA1234
```
# 暂存(Staging)
## 8.我需要把暂存的内容添加到上一次的提交(commit)
```
git commit --amend
```
## 9.我想要暂存一个新文件的一部分，而不是这个文件的全部
一般来说, 如果你想暂存一个文件的一部分, 你可这样做:
```
git add --patch filename.x
```
-p 简写。这会打开交互模式， 你将能够用 s 选项来分隔提交(commit)；然而, 如果这个文件是新的, 会没有这个选择， 添加一个新文件时, 这样做:
```
git add -N filename.x
```
然后, 你需要用 e 选项来手动选择需要添加的行，执行 git diff --cached 将会显示哪些行暂存了哪些行只是保存在本地了。

## 10.我想把在一个文件里的变化(changes)加到两个提交(commit)里
git add 会把整个文件加入到一个提交. git add -p 允许交互式的选择你想要提交的部分.
## 11.我想把暂存的内容变成未暂存，把未暂存的内容暂存起来
多数情况下，你应该将所有的内容变为未暂存，然后再选择你想要的内容进行commit。但假定你就是想要这么做，这里你可以创建一个临时的commit来保存你已暂存的内容，然后暂存你的未暂存的内容并进行stash。然后reset最后一个commit将原本暂存的内容变为未暂存，最后stash pop回来。

```
git commit -m 'WIP'
git add.
git stash
git reset HEAD6
git stash pop --index 0
```
注意1: 这里使用pop仅仅是因为想尽可能保持幂等。注意2: 假如你不加上--index你会把暂存的文件标记为为存储
# 未暂存(Unstaged)的内容
## 12.我想把未暂存的内容移动到一个新分支
```
git checkout -b mybarch
```
## 13.我想把未暂存的内容移动到另一个已存在的分支
```
git stash
git heckout mybrach
git stash pop
```
## 14.我想丢弃本地未提交的变化(uncommitted changes)
果你只是想重置源(origin)和你本地(local)之间的一些提交(commit)，你可以：
```
#one commit
git reset --hard HEAD^

#two commit
git reset --hard HEAD^^
#four commits
git reset --hard HEAD~4
#ALL
git checkout -f
```
重置某个特殊的文件, 你可以用文件名做为参数:
```
git commit filename
```
## 15.我想丢弃某些未暂存的内容
如果你想丢弃工作拷贝中的一部分内容，而不是全部。

**签出(checkout)不**需要的内容，保留需要的。
```
git checkout -p
```
另外一个方法是使用 **stash**， Stash所有要保留下的内容, 重置工作拷贝, 重新应用保留的部分。
```
git stash -p
#select all of the snippets you want to save
git reset --hard
git stash pop
```
或者, **stash** 你不需要的部分, 然后stash drop。

```
git stash -p
#select all of nippets you don`t want to save 
git stash drop
```
# 分支(Branches)











