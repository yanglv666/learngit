## 创建版本库

`pwd`命令用于显示当前目录

通过`git init`命令把这个目录变成Git可以管理的仓库

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

####把一个文件放到Git仓库只需要两步：

第一步，用命令`git add`告诉Git，把文件添加到仓库

第二步，用命令`git commit`告诉Git，把文件提交到仓库

（`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。）

## 时光机穿梭

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

#### 版本回退

`git log`命令显示从最近到最远的提交日志（如果嫌输出信息太多，看得眼花缭乱的，可以试试加上

`--pretty=oneline`参数）

在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

我们要把当前版本回退到上一个版本，就可以使用`git reset`命令：`$ git reset --hard HEAD^`

用`git reset -- hard 版本号` 可以回到任何一个你想回到的版本

Git提供了一个命令`git reflog`用来记录你的每一次命令

#### 工作区和暂存区

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

用`git status` 可以查看当前状态

#### 管理修改

Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用``git diff HEAD -- 文件名`命令可以查看工作区和版本库里面最新版本的区别

#### 撤销修改

Git会告诉你，`git checkout -- file（文件名）`可以丢弃工作区的修改：

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

Git同样告诉我们，用命令`git reset HEAD file（文件名）`可以把暂存区的修改撤销掉（unstage），重新放回工作区

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

#### 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm -r  `命令删了

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm -r`删掉，并且`git commit`

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
$ git checkout -- file(文件名)
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

## 远程仓库

#### 添加远程库

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`

例如：`$ git remote add origin git@github.com:yanglv666/learngit.git` 即可关联我账户上的learngit远程库

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

把本地master分支的最新修改推送至GitHub

##### 用git命令创建远程分支：

```
创建分支        $ git branch 分支名
推送到远程      $ git push origin 分支名
```

用`git branch -a` 可以查看本地和远程的分支情况。

#### 从远程库克隆

用命令`git clone`克隆一个本地库

```
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

$ cd gitskills
$ ls
README.md

```

注意把Git库的地址换成你自己的，然后进入`gitskills`目录看看，已经有`README.md`文件了。

## 分支管理

#### 创建与合并分支

查看分支：`git branch` （`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。）

创建分支：`git branch <name>`

切换分支：`git checkout <name>` 

创建+切换分支：`git checkout -b <name>` 

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

合并某分支到当前分支：`git merge <name>` （`git merge`命令用于合并指定分支到当前分支。）

删除分支：`git branch -d <name>`

大致步骤为：先创建并切换到新分支，然后对文件进行修改后并提交，再切换回原分支，然后将新分支合并到原分支上，最后删除新分支。

#### 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用`git log --graph`命令可以看到分支合并图:

```
$ git log --graph --pretty=oneline --abbrev-commit
*   59bc1cb conflict fixed
|\
| * 75a857c AND simple
* | 400b400 & simple
|/
* fec145a branch test
...
```

#### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```
$ git merge --no-ff -m "merge with no-ff" dev
```

#### Bug分支

用 `git stash`  ，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支

然后，修复bug，

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支。

Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```

附：

`git stash`: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。
`git stash pop`: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复。
`git stash list`: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。
`git stash clear`: 清空Git栈。此时使用gitg等图形化工具会发现，原来stash的哪些节点都消失了。

#### Feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

#### 多人协作

要查看远程库的信息，用`git remote`

或者，用`git remote -v`显示更详细的信息

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

##### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```
$ git push origin dev
```

##### 抓取分支

`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送

- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

取消本地分支和远程分支的链接关系，用命令`git branch --unset-upstream branch-name  ` 

