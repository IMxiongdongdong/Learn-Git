# `git`学习

`git`跟踪管理的是修改，而非文件。

工作区：就是在电脑里能看到的目录。
版本库：即隐藏目录`.git`，版本库里存了很多东西，其中最重要的就是称为`stage`或者叫`index`的暂存区，还有自动创建的第一个分支`master`以及指向`master`的一个指针叫`HEAD`。

## 1. 添加命令

### `git add`

 `git add -A`

  提交所有变化。

`git add -u`

  提交被修改和被删除的文件，不包括新文件。

`git add . `

  提交新文件和被修改的文件，不包括被删除的文件。

`git add readme.txt`

  提交指定文件。

`git add file1.txt`

`git add file2.txt`

`git add file3.txt`

  可以同时添加几个文件。

## 2. 提交命令

### `git commit`

`git commit -m "message"`

  `message`代表提交说明，可以输入任何内容 。

`git commit --amend`

有时候提交过代码之后，发现一个地方改错了，下次提交时不想保留上一次的记录，或者上一次的`commit message`的描述有误，可以使用这个命令。

想修改描述信息的话，直接键入`:i`，此时进入了输入模式，可以用键盘上下键转到描述所在的那一行，然后进行修改。修改完成后按`esc`退出编辑模式，在键入`:wq`回车退出并保存修改。

## 3. 查看状态

### `git status`

掌握仓库当前的状态

## 4. 重命名文件

### `git mv`

对已经添加到暂存区的文件重命名

## 5. 查看修改

### `git diff`

`git diff readme.txt`查看difference。

## 6. 查看日志

### `git log`

`git log`可以显示最近到最远的提交日志。

`git log --pretty=oneline`

可以简略好看的显示日志。

## 7. 版本回退

### `git reset`

`HEAD`指向的版本就是当前版本，`git`允许在历史版本之间穿梭，使用命令`git reset --hard commit_id`。版本回退的速度非常快，因为`HEAD`是一个指向当前版本的指针，当回退版本时，仅仅是把`HEAD`换了指向。`commit_id`没必要写全，写前几位就可以了，`git`会自动去找，当然也不能致只写前一两位，因为`git`会找到多个版本号，就无法确定是哪一个了。

`git reset --hard`是彻底回退到某个版本，本地的源码也会变成上一个版本的内容。

`git reset --soft`是回退到某个版本，只回退了`commit`的信息，如果还要提交，直接`commit`即可。

用`git log`查看提交历史，以便确定回退到哪个版本。

`git reflog`查看命令历史，忘掉commit_id时可以用到。

### `git revert`

`reset`的做法是移动分支指针到`commit`链的其他位置实现撤销更改，`revert`的命令会在链的末尾添加新的提交以取消更改。

## 8. 删除文件

### `git rm`

`git rm `

同时从工作区和索引中删除文件，本地的文件也会删除

`git rm --cache`

从索引中删除文件，但是本地文件还在，只是不希望这个文件被版本控制。

## 9. 撤销修改

### `git checkout`

当改乱了工作区某个文件的内容，想直接丢弃工作区的修改是，用`git checkout -- readme.txt`，让这个文件回到最近一次`git commit`或者`git add`时的状态。

当改乱了工作区某个文件的内容，还添加到了暂存区，想丢弃修改，第一步用`git reset HEAD readme.txt`,回到上面，在进行上面的操作。

## 10. 远程推送

### `git push`

关联一个远程仓库，使用命令`git remote add origin git@server-name:path/repo-name.git`

第一次推送`master`分支的所有内容：`git push -u origin master`。`origin`代表的是远程库。

如果之前在远程仓库里新建了`README.MD`文件，会导致推送失败，要先执行拉取命令`git pull --rebase origin master`。

`git remote`

查看远程库信息。

`git remote -v`

显示更详细的信息。

`git push origin master`

推送到远程库分支。

`git push origin dev`

推送到其他分支。

`git pull origin master`

本地仓库与远程同步。

* 利用码云加速克隆：在码云上选择从`github`导入仓库，然后克隆码云的项目地址，最后将本地的项目关联到原来的项目上去，打开`.git`文件夹中的`config`将`[remote"origin"].url`字段重新关联到原来的`github`项目地址。

`git pull`与`git fetch`在功能上是大致相同的，都是起到了更新代码的作用。

* `git fetch`相当于从远程获取最新版本到本地，不会自动`merge`。
* `git pull`相当于是从远程获取最新版本并`merge`到本地。

## 11. 克隆

通过`ssh`支持的原生`git`协议速度最快。

## 12. 分支管理

`git branch dev`

创建名字叫做`dev`的分支。

`git checkout dev`

切换到分支`dev`。

`git branch -m dev dev1`

更改分支名称`dev`为`dev1`。

`git branch`

查看当前分支。

`git merge dev`

合并指定分支到当前分支。

`git branch -d dev`

删除分支。

`git log --graph`查看分支合并图。

通常合并分支时，如果可能`git`会采用`fast forward`模式，但这种模式下，删除分支后会丢掉分支信息。`fast forward`是快进合并，是最理想的合并。

强制禁用`fast forward`模式，`git`会在`merge`时生成一个新的`commit`，这样从分支历史上就可以看出分支信息。

`git merge --no-ff -m "merge with no-ff" dev`

`--no-ff`参数，表示禁用`fast forward`模式。

### `git diff`

用来进行比较文件的不同，即比较文件在暂存区和工作区的差异。显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别。

`git diff`

尚未缓存的改动。

`git diff --cached`

查看已缓存的改动。

`git diff HEAD`

查看已缓存的与未缓存的所有改动。

`git diff --stat`

显示摘要而非整个`diff`。

## 13. 储存现场

### `git stash`

可以把当前工作现场先储存起来，等以后恢复现场后再继续工作。

`git stash list`

查看工作现场。

`git stash apply`

恢复工作现场，但是`stash`内容不会删除，需要用`git stash drop`来删除。

`git stash pop`

恢复的同时把`stash`内容也删了。

## 14. 标签管理

### `git tag`

首先切换到需要打标签的分支上，然后`git tag v1.0`，就给分支打上了`v1.0`的标签。

`git tag`

可以查看所有标签。

`git tag -d v0.1`

删除标签。

创建的标签都只存储在本地，不会自动推送到远程。

`git push origin tagname`

推送一个本地标签。

`git push origin --tags`

推送全部未推送过的本地标签。

`git tag -d tagname`

删除一个本地标签。

`git push origin :refs/tags/tagname`

删除一个远程标签。