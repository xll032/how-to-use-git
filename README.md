### Git 学习笔记

>*参考教程： [Git教程 - 廖雪峰](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000")*

>*简单的一个学习笔记，详细的教学可以看上面的链接，基本上学会这些就能正常使用 `git` 了。*
#### 1. 创建版本库
```
mkdir test

cd test

# 显示当前目录
pwd  

# 初始化当前目录为git仓库，会有一个隐藏的 .git 目录（用于跟踪管理版本库，不要去改它）
git init 
```

#### 2. 添加、提交文件（要先 `add`，再 `commit`）
```
# 创建一个文件（<fileName>） 内容随意
    
# 把文件加入到 git 中（相当于 svn 的添加）
git add <fileName>  

# 提交文件到仓库中（相当于 svn 的提交）
git commit -m "说明文字" 
```

#### 3.查看仓库的状态
```
git status
```

#### 4. 查看文件的区别
```
git diff <fileName>
```
 
#### 5.查看修改日志
```
git log
# 会多行显示内容，如果希望更加清晰简洁，可以使用 git log --pretty=oneline  
# 显示的一长串字符是版本号的意思
```

#### 6. 版本回退 
```
git reset --hard HEAD^
```

* `HEAD` 表示当前版本，`HEAD^` 表示上一个版本，每多一个 `^` 就表示往上一个版本，
`HEAD~100`可以表示多个版本（向上100个版本）

* 这时候查看记录就会找不到之后的版本了，就像是时光倒流一样，如果记得未来的版本号，还是可以回去。原来窗口里能看到。使用:
 
```
git reset --hard b722f070ac84 # 版本号，可以不写全但不能写的太少，git会自己去找）
```
* 假如没有记住版本号，还是有办法找回的。 使用 git reflog 可以看到每一次的版本号，使用相应id就可以恢复了。

#### 7. `git`暂存区概念
              add               commit
    本地文件   -->   放入暂存区    -->    提交到仓库，并清空暂存区

* 如果修改了文件，没有 add ，就不会被 commit 提交

* 如果修改了多次，可以多次add，一起合并commit

#### 8. 丢弃工作区的修改（`checkout` 就是 `svn` 中的检出）
```
git checkout -- <fileName>  # 丢弃工作区的修改，checkout 也可以用于切换分支
```
简单的说，就是回到最近一次 `git commit` 或 `git add` 时的状态

#### 9. 撤销 `add` 而没有 `commit` 的文件
```
git reset HEAD <fileName> # 将已放入暂存区的文件撤销（撤销 git add 的状态）
```

#### 10. 撤销 `commit`
```
git reset --hard HEAD^  # 即回退版本
```
但如果已经提交到远程版本库。。那就真的是提交了。。（别人就能看到）

#### 11. 删除文件

如果是误删了本地文件，可以使用 
```
git checkout -- <fileName> （从仓库里检出文件）
```
的确要删除的话，使用 `git rm <fileName>` 就可以了（相当于 `add`，之后也需要 `commit`）
如果删除后，还是提交了也可以恢复，但是会丢失最近一次提交后修改的内容

#### 12. 本地关联远程仓库
* 首先在 `github` 上建一个新的仓库，完成后有提示如何将本地文件关联到 `github` 上
* 关联操作：
```
git remote add origin https://github.com/<userName>/<repositoryName>.git
```
* 首次提交：
```
git push -u origin master # 之后 commit 可以省略 -u
```
* 这是 `git` 相较于 `svn` 的一个非常大的优势。
* `git` 可以在不联网时正常工作，之后只需要在有网络的时候推送到远程仓库同步就好了。
* 而 `svn` 在无网络情况下无法使用。

#### 13. 克隆远程仓库
```  
git clone https://github.com/<userName>/<repositoryName>.git # clone 项目地址
```

#### 14.创建分支
* `svn` 的分支切换和创建非常慢。。所以大家都不喜欢用，导致这个强大的功能形同虚设。
* `git` 的分支切换、创建、删除都是非常快的。
```
git checkout -b <devName> # 创建一个名为 <devName> 的分支并切换 
``` 
相当于：
```
git branch <devName> # 创建分支
git checkout dev # 切换分支
```
```
git branch  # 查看所有分支  当前分支前有一个 *
```
```
git merge <devName> # 合并分支
```
```
git branch -d dev # 删除分支
```

#### 15. 解决冲突
* 不同分支修改了相同地方会产生合并冲突，会显示（只是一个例子）：
```    
<<<<<<< HEAD
master 修改的内容
=======
branch 修改的内容
>>>>>>>
```
* 可以直接打开文件选择需要的内容，然后提交
```
git log --graph --pretty=oneline --abbrev-commit  # 查看日志的图形
```

#### 16. 分支管理策略
* 合并分支时，`git` 会尽可能使用 `Fast forward` 模式，这种模式会在分支删除后，丢失分支信息。
* 强制禁止分支模式可以使用参数 `--no-ff`，这样就会在 `merge` 时生成一个新的 `commit`，这样就能从历史中看到分支信息
* 实际使用： 
```
git merge --no-ff -m "merge with no-ff" <devName>
```
* 这样一来，就可以保留分支信息。
实际开发中应该按照下面几个原则进行：
* 保证 `master` 的稳定，仅用来发布新版本，平时工作都在 `dev` 上进行
* `dev` 分支是不稳定的，需要发布版本的时候，将其 `merge` 到 `，master` 上
* 每一个小伙伴都有自己的分支，需要的时候去主干上合并就可以了

#### 17. `bug` 分支
* 每一个 `bug` 都可以通过一个分支来修复，完成修复后合并即可。
* 当你在某一个分支的工作还没有完成，还不能提交，而此时需要修复一个 `bug`，`git` 也提供了办法。

假定目前在 `dev` 上开发，`add` 之后不 `commit`（因为未完成）。
* 首先保存当前的工作现场
```
git stash # 保存工作现场
```
* 然后从主干创建一个 `bug` 分支
```
git checkout master # 切换到主干
git checkout -b issue-01 # 创建分支 issue-01 并切换到该分支
```
* 修复好之后，提交并合并到主干
```
git add <fileName> # 添加改好的文件
git commit -m "fix bug 01" # 提交
git checkout master # 切换到主干
git merge --no-ff -m "merge fixed bug 01" issue-01 # 合并修复好的 bug
git branch -d issue-01 # 删除该分支
```
* 完成 `bug` 的修复后，回到 `dev` 去干活
```
git checkout dev # 切换到分支 dev
git status # 查看一下状态，这一步不需要
git stash list # 查看保存的工作现场，这一步也可以不需要
git stash apply # 恢复现场，并不删除 stash 内容
git stash drop # 删除 stash 内容
```
`git stash apply` 和 `git stash drop` 也可以用 `git stash pop` 来代替。可以用 `git stash list` 看看。
`git stash` 也可以多次使用，恢复的时候需要带上版本，就像这样：
```
git stash apply stash@{0}
```
简单来说，就是手上有工作没有完成，而要去修复 `bug`，可以先 `git stash` 保存，然后回去修复，完成后，使用 `git stash pop` 回到现场。

#### 18. feature 分支
开发中，肯定会有很多新功能添加进来，这时候们可能会有一些实验性的代码，我们并不希望这些代码把主干分支中的代码弄乱了。所以在添加新功能的时候，最好建一个 `feature` 分支，完成后再合并，并删除这个 `feature` 分支。
创建 `feature` 分支本质是和 `bug` 分支类似的。就不展开写相关操作了。
强制删除分支可以使用 `-D` 参数。

#### 19. 多人协作

 