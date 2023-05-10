

# Git 基础

## 配置用户信息

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com

//这样设置了全局配置，如果不想要设置全局，不用加 --global
```

问题： 如果设置了全局用户信息，想要重新更改怎么办？

```
git config --list //检查所有得配置信息
git config user.name //检查某个配置信息
```



## git   help

```
git config -h //查看config参数
git add -h //查看add参数
```



## 获得git仓库

```
1 本地初始化一个仓库
2 克隆一个远程仓库

1 本地初始化一个仓库
    git init
    git remote add <url-name> <url> //连接到远程仓库
    git add .
    git commit -m 'message'
	

2 克隆现有得仓库
	git clone <url>
	git clone <url> <name> //克隆的同时给本地仓库取名
```



## 记录每次更新到仓库

```
git status //检查当前文件状态
git add <file-name> //add具体的文件到暂存区
git add . //add所有的文件到暂存区

git status 只能每次看到文件的状态，不能看到文件具体被修改了什么
git diff //可以看到文件具体被修改了什么

--cached / --staged //在git中都表示暂存区的意思
```



## 忽略文件

```
.gitignore 文件

语法：

    # 忽略所有的 .a 文件
    *.a

    # 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
    !lib.a

    # 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
    /TODO

    # 忽略任何目录下名为 build 的文件夹
    build/

    # 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
    doc/*.txt

    # 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
    doc/**/*.pdf
```

![image-20230425234141892](C:\Users\李强\AppData\Roaming\Typora\typora-user-images\image-20230425234141892.png)



## 移除文件

```
1 如果在本地库中删除了一个文件，需要告诉git，否则git不知道，依然会追踪这个文件

git rm <file-name> 
git rm -f <file-name> //如果文件已经放入暂存区，需要强制删除追踪

2 另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话	说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 .gitignore 文件，不小心把一个很大的日志文	件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 --cached 选项：

git rm --cache <file-name>
```



## 移动/重命名文件

```
mv 命令，所以叫移动命令，但是也可以用来重命名文件

git mv <原文件路径> <目标文件路径>

git mv old_name new_name //重命名一个文件
git mv file.txt docs/  //将名为 file.txt 的文件移动到目录 docs/ 中
```



## 查看提交历史

```
git log //查看提交历史，最上面的是最新的

git log -p //显示每次提交的变化
git log -p -2 //显示最近两次提交的差异

git log --stat //显示每次提交的总结，增加了或删除了多少文件等

git log --pretty=oneline //一行显示提交记录
git log --pretty=short //简短显示
git log --pretty=full //详细显示
git log --pretty=fuller //更加详细显示

$ git log --pretty=format:"%h - %an, %ar : %s" //
```



## 撤销操作

```
1 最终你只会有一个提交——第二次提交将代替第一次提交的结果

	$ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend
    
2 取消暂存的文件
	git restore --staged <file-name>
	
3 撤销对文件的修改
	git restore <file-name>
```



## 远程仓库的使用

```
git remote add <url-name> <url> //连接到远程仓库并给远程仓库起名
git fetch <url-name> //拉取远程仓库的代码但是不会合并
git pull <url-name> //拉去代码并合并

git push <remote> <branch> //从本地分push到远程仓库

git remote 
git remote -v

git remote rename <old-name> <new-name> //重命名远程仓库名

git remote rm <name> //移除远程仓库
```



## 打标签

```

```



## git 别名

```
git config alias.ch checkout //在当前仓库中给checkout取名为ch 
git config alias.br branch

alias: 化名

//如果想要全局设置
git config --global alias.ch chekout //添加 --global
```



## 全局配置

```
删除全局配置

git config --global --unset-all user.name
git config --global --unset-all user.email

重置全局配置

git config --global user.name "Jane Doe"
```



# Git 分支

## 使用分支

```
git branch A //创建一个新分支branchA

git checkout A //切换到分支A

git checkout -b A //创建新分支A并切换到A

git branch -m <new-name> //给分支名重命名
```



## 分支的新建与合并

```
让我们来看一个简单的分支新建与分支合并的例子，实际工作中你可能会用到类似的工作流。 你将经历如下步骤：

    1 开发某个网站。
    2 为实现某个新的用户需求，创建一个分支。	
    3 在这个分支上开展工作。

正在此时，你突然接到一个电话说有个很严重的问题需要紧急修补。 你将按照如下方式来处理：

    1 切换到你的线上分支（production branch）。
    2 为这个紧急任务新建一个分支，并在其中修复它。
    3 在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。
    4 切换回你最初工作的分支上，继续工作。
```

```
主分支 master 

切换到分支home上开发home页面
git checkout -b home

收到紧急通知，先开发user页面
	1 暂存A分支上home的开发  git stash
	2 切回master git checkout master
	3 新开分支user，并开发user页面 git checkout -b user
	4 user页面开发完毕，提交 git add / git commit
	5 切回到home页面，返回当时暂存的状态 git checkout home / git stash apply

继续开发home页面
	1 开发完毕，提交 git add / git commit 

合并分支
	1 切回主分支合并  git checkout master / git merge home
	2 合并user分支  git merge user
	
	
//git stash 命令后面会说到
```



## 分支管理

```
git branch //列出所有本地分支
git branch -v //列出所有本地分支最近的提交
git br -d test //删除test分支，前提是test分支的代码修改全部提交了

git branch --merged //查看哪些分支已经合并到当前分支
git branch --no-merged //查看没有合并到当前分支的分支
```



## 绑定远程仓库分支

```
git branch -u <remote-name>/<remote-branch> //将当前分支绑定远程仓库分支

git push -u <remote-name> <local-branch>:<remote-branch> //推送代码并绑定
git push -u <remote-name> <branch>	//省略远程分支名（本地分支和远程分支名一样）

* 如果远程分支不存在，需要先使用 git push 命令将本地分支推送到远程仓库，并创建一个新的远程分支。

git branch -vv //查看本地分支和远程分支的联系
git branch -u <> <> //重新修改绑定关系
git branch --unset-upstream <local-branch>  //解除绑定
```

```
* 如果远程仓库中没有子分支

git push -u origin <local-branch>  //将会在远程仓库中新建一个和本地分支同名的分支并绑定，然后推送代码
```



## 分支开发工作流

```

```



## 变基

```
问题：当前在master分支上，现在需要开发home页面

	1 git checkout -b home //新开分支home并切换
	2 git add . / git commit -m //在home分支修改完后提交
	3 git checkout master //切回master分支
	4 git rebase home //rebase home分支到master分支
	5 git push  //直接就可以在master分支上push，不用merge后再commit

优点：如果使用merge合并后再commit，就多了一次commit历史，直接将home rebase to master，就只需要在
		home 分支上commit一次，master分支就不用commit了，使用rebase类似合并代码并合并提交，省略了一次
		提交。
```



![image-20230426150816409](C:\Users\李强\AppData\Roaming\Typora\typora-user-images\image-20230426150816409.png)

```
$ git rebase --onto master server client

问题：变基有风险，具体看git的官网教程，我还没有整理
```



# 分布式Git

## 分布式工作流程

```
1 集中式工作流程
	比较简单，和传统的管理工具差不多
	
2 集成管理者工作流程
	目前常用的，GitHub gitlab 经常用

3 主管与副主管工作流
	超大型项目用
```



# 实际开发

```
1 先拉去远程仓库主分支的代码，保证本地仓库的代码最新
2 在远程仓库中新建一个属于自己的分支，后面的代码push到自己分支
3 本地仓库分支与远程仓库自己分支关联

可以直接在git上给远程仓库新建属于自己的分支
```



# 常用命令



```
git restore <filename>  //恢复文件
git restore --staged <filename> //取消暂存状态

git rm <filename> //删除文件
git rm <filename> -f //强制删除

git mv <oldname> <newname> //重命名文件
git mv from to  //移动文件
```



## git checkout



用于在本地仓库中切换分支或还原文件。



```
1 切换分支：使用以下命令切换到指定分支：
git checkout <branch-name>


2 创建并切换到新分支：使用以下命令创建新分支并切换到该分支：
git checkout -b <new-branch-name>


3 还原文件：将文件还原为最近一次提交的版本
git checkout --<filename>


4 切换到指定提交：使用以下命令切换到指定提交的状态，该命令会将当前分支切换到“游离状态”：
git checkout <commit-SHA>


5 切换到远程分支：使用以下命令切换到远程分支，该命令会在本地创建一个新的分支，并将其与远程分支关联：
git checkout -b <local-branch-name> origin/<remote-branch-name>
```



# 提交代码到仓库中



## 个人开发



## 团队开发



```
git add
git commit -m ''
git pull 分支名  仓库名
git add
git commit -m ''
git push 分支名  仓库名

/*
	git add  git commit 没什么好说的
	但是是在团队开发，所以在push自己的代码之前，需要检查当前仓库是否有别的同事提交了新的代码
	如果有同事提交了新的代码，则就产生了冲突，需要解决冲突，一般都是分别开发不同的功能，所以就是
	合并同事的代码，解决了冲突后，因为合并了新代码，所以需要重新提交，就再来一次 add commit 最后
	push
	
	时刻用git status 查看状态
*/
```



```markdown
* 注意  git commit -m '' 必须要加提交说明，否则提交失败，abort，提交中止
```



# 代码撤销



有两个命令， `git reset` `git revert`



## git reset



用于撤销提交、修改 commit 历史、取消暂存的文件等操作。常见的用法有：

1. `git reset HEAD <file>`：取消暂存指定的文件，让它回到工作区。
2. `git reset <commit>`：将当前分支的 HEAD 指针指向指定的 commit，并取消这个 commit 之后的所有提交，即将这些提交的更改回滚掉。
3. `git reset --hard <commit>`：与上一个命令类似，但会将当前分支的工作区和暂存区的文件也一起回滚，即将所有更改都撤销掉，慎用。
4. `git reset --soft <commit>`：将当前分支的 HEAD 指针指向指定的 commit，但不取消之后的提交，即将这些提交的更改保留在暂存区中，可以继续修改并重新提交。

需要注意的是，使用 `git reset` 命令会修改 Git 的历史记录，因此在操作时需要谨慎，避免误操作导致数据丢失。建议在操作前先备份重要的数据，或者在使用 `git reset` 命令时加上 `-n` 参数，先预览将要回滚的更改，确认无误后再执行回滚操作。



### git reset HEAD  file



假设你已经添加了文件 `file1.txt` 和 `file2.txt` 到暂存区，但是后来发现 `file1.txt` 不应该被提交，需要将其从暂存区中移除，可以使用以下命令：



```
git reset HEAD file1.txt
```



这样就将 `file1.txt` 从暂存区中移除了，同时保留了工作区的修改。接着，你可以使用 `git add` 命令将修改重新添加到暂存区，或者使用 `git checkout` 命令丢弃这些修改。



### git reset --soft



这个命令的作用不删除工作区，也不删除暂存区的数据，它就是删除 commit 历史，但是数据还在，比如你比较菜，一直修改一直 commit，结果 push 一次一看几十个 commit，这个时候可以用这个命令指定到最初的 commit，但是保留我们修改的数据，然后 push,就删除了很多个 commit 历史，当然自己本地 git 是可以看到的，但是GitHub 仓库上别人是看不到你的 commit 历史，只会看到你一次 commit 就修改了这么多数据，牛逼。



假设当前的 Git 提交历史为 A - B - C - D，现在想要撤销掉最近一次的提交 D，但是保留修改内容，可以使用以下命令：



```
git reset --soft HEAD~1
```



这个命令将会把当前分支的 HEAD 指针移动到上一个提交 C 上，并将提交 D 中所做的修改移动到暂存区中，这样就可以重新提交了。使用 `git status` 命令可以查看暂存区和工作区的状态。



### git reset --mixed



它介于 --soft 和 --hard 之间

-- soft 是工作区和暂存区数据都保留，只删除 commit 历史，--hard 是全部都不保留，一夜回到解放前。

--mixed 是保留工作区的数据修改，不保留暂存区的修改，如果想继续 push 修改的数据，则重新 add commit



### git reset --hard



和 --soft 有所不同的是，比如你一直修改一直 commit，但是最终发现还是最初的代码比较好，不想手动修改了，想直接回到最初的代码，就用这个命令，不保存你的数据，工作区（本地）和 暂存区都不保存你的修改数据，直接回到最初的样子



就像是ps设计，第一版，第二版。。。第六版，这个时候觉得还是第一版好，直接后面的全都不要了，就要第一版



一般不推荐使用，因为这种太极端，容易不可逆的丢失数据，如果想进行这种操作，也提前备份好数据。



### 举例

```
//我commit 很多次，但是 push 的时候只想显示一次 commit

git log
git reset --soft commit_id
git add .
git commit -m 'message'
git pull 分支名 仓库名
解决冲突
git add .
git commit -m 'message'
git reset --soft commit_id
git add .
git commit -m 'message'
git pull 
git push 分支名 仓库名
```



## git revert



# Git 插件



# 分支



在公司开发中，pull 公司的代码后，第一件事就是新建一个自己的分支，然后在自己的分支上写代码，这样无论自己的代码写的多菜，都不会影响总分支

在自己的分支上开发新的功能，也可以给自己开一个分支，在新分支上开发功能，开发的没错在合并到自己的分支上



## 新建分支



```
git branch 				查看当前分支
git branch 分支名		  新建分支
git branch -d 分支名	  删除分支
git switch 分支名		  切换分支
git switch -c 分支名	  新建并切换分支（需要新版本的git，老版本的可能不支持）
```



## 合并分支



分两种情况合并，一种是直接快速合并，一种是需要解决冲突合并



### 快速合并



![image-20230421105437188](C:\Users\李强\AppData\Roaming\Typora\typora-user-images\image-20230421105437188.png)



如上图，主分支是 master，一开始有两个提交历史，分别是 commit1 commit2，在commit2 节点新建了一个分支 update，除了 update 分支没有其它的子分支，这个时候 update 开始了两个更改并提交，修改完代码后，update 分支完成任务，则将 update 修改的代码合并到 master 分支，因为没有别的子分支，所以不会产生冲突，所以直接快速合并，合并完 master 分支就到了 commit4



```
* master
git commit -m 'commit1'
git commit -m 'commit1'
git switch -c update    //新建update分支并切换

* update
git commit -m 'commit3'
git commit -m 'commit4'

*切换到master合并update
git switch master
git merge update   //合并分支
```



### 解决冲突合并



# 项目中使用git的场景



## 需求开发前的分支拉取流程？



1. 创建主分支：通常情况下，主分支是用于稳定发布的分支，如`master`分支。
2. 创建开发分支：从主分支创建一个新的分支，如`develop`分支，用于开发新需求。
3. 拉取远程分支：如果你和团队成员一起协作开发，你可能需要从远程仓库拉取最新的主分支和开发分支。

```
git fetch origin master
git fetch origin develop
```

1. 切换到开发分支：使用以下命令切换到本地的开发分支：

```
git checkout develop
```

1. 开发新需求：在本地开发分支上进行新需求的开发。可以添加、修改或删除代码，并提交更改。
2. 推送到远程分支：当你完成了新需求的开发，你可能需要将本地分支推送到远程仓库中，以便与其他团队成员共享代码。可以使用以下命令将本地分支推送到远程分支：

```
git push origin develop
```



## 需求开发后的分支合并流程？



## 分支合并出现冲突如何解决？



## 出现线上问题时hotfix分支的操作流程？



## 线上出现事故代码如何回退？





bfcfe73a5bbbacf1c7f46d2d14e66019863a5b0a