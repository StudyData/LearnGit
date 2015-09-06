1、`git init`					  			创建版本库
2、`git add readme.txt` 					添加readme.txt到仓库
3、`git commit -m '修改说明'` 				提交文件到仓库， “-m”后面输入的是本次提交的说明

4、	`$ git add file1.txt` 				添加一个文件到仓库
   		`$ git add file2.txt file3.txt` 		一次添加多个文件到仓库
		`$ git commit -m "add 3 files."`      提交当前的所有修改


5、`git status`  							查看仓库当前状态

6、`git diff` 								查看difference

7、`git log`								查看提交的历史记录

	git log --pretty=online				单行显示历史记录
	git log --graph						命令可以看到分支合并图
	git log --graph --pretty=oneline --abbrev-commit	查看分支合并图

8、`git reset --hard HEAD^`				回退到上一个版本，上上一个版本就是HEAD^^，当然往上100个版本写成HEAD~100。

9、`git reset --hard 3628164`				回退到指定版本。版本号没必要写全，前几位就可以了，Git会自动去找

10、`git reflog` 							查看命令历史，以便确定要回到未来的哪个版本。

11、`git checkout -- file	`				丢弃工作区的修改,实际是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
			命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
			一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
			一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
			注： `git checkout -- file`命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令

12、`git reset HEAD file`					可以把暂存区的修改撤销掉（unstage），重新放回工作区

13、`git rm file` 							从版本库中删除file文件

14、`git remote add origin https://github.com/xx-li/learngit.git` 		从这个仓库克隆出新的仓库，或者把一个已有的本地仓库与之关联

15、`git push -u origin master`			把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

			加上了`-u`参数，Git不但会把本地的master分支内容推送到远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

			关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；
			此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

16、`git clone git@github.com:xx-li/gitskills.git` 	从远程库克隆一个本地库

			还可以用https://github.com/xx-li/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。
			使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。

###创建与合并分支
* 查看分支 
	* `git branch`
* 创建分支 
	* `git branch <name>`
* 切换分支 
	* `git chekout <name>`
* 创建+切换分支 
	* `git checkout -b <name>`
* 合并某分支到当前分支 
	* `git merge <name>`
* 删除分支 
	* `git branch -d <name>`
	* `git branch -D <name>` 强行删除

###解决冲突
* 当分支之间都有新的提交， 并且各自的修改是冲突的，在执行`git merge featurel`时是会提示冲突的。
* 当出现这个问题的时候我们需求手动修改冲突文件，`git status`可以查看冲突的文件，修改文件后可以重新提交

###分支管理策略
* 普通模式合并
	* `git merge --no-ff <name>`
	* 实例：`git merge --no-ff -m "merge with no-ff" dev`  "merge with no-ff"是提交的log
	* 普通模式合并，合并后的历史有分支，能看出曾经做过合并。 直接`git merge <name>`看不出曾经做过合并

###bug分支
* 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
* 保存当前工作现场（等以后回复现场后继续工作）
	* `git stash`
* 查看保存的工作现场列表
	* `git stash list`
* 删除stash
	* `git stash drop`
* 恢复工作现场，并且删除stash
	* `git stash pop`
* 恢复指定的工作现场
	* `git stash apply stash@{0}`

###多人协作
* 推送自己的修改
	* `git push origin <name>`
	* `git pull`   如果推送失败，则因为远程分支比你的本地更新，需要使用此命令合并
	* 如果`git pull`提示`no tracking information`,则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`
	* 如果合并有冲突，则解决冲突，并在本地提交
* 查看远程库信息
	* git remote -v
* 本地推送分支
	* `git push origin <name>` 如果推送失败，先用`git pull` 抓取远程的最新提交 
* 在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致；
	* `git checkout -b branch-name origin/branch-name`
* 建立本地分支和远程分支的关联
	* `git branch --set-upstream branch-name origin/branch-name`
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；

###标签
* 创建标签
	* `git tag v1.0`   打一个v1.0的新标签
	* `git tag`		   查看所有的标签，注意标签不是按时间顺序列出，而是按字母排序的
	* `git tag v0.9 6224937`   针对历史提交版本“6224937”打tag
	* `git show v0.9`	查看指定标签信息
	* `git tag -a v0.1 -m "version 0.1 released" 3628164`  创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字，`3628164`指定commit ID
	* `git tag -d v0.1`	删除指定标签
	* `git push origin <tagname>`	推送指定标签到远程
	* `git push origin  --tags`		一次性推送全部尚未推送到远程的本地标签
	* 删除已经推送到远程的标签
		* `git tag -d v0.9`	先本地删除
		* `git push origin :refs/tags/v0.9`	然后远程删除

###自定义Git
* `git config --global color.ui true` 让git显示颜色

###忽略特殊文件
* 忽略某些文件时，需要编写`.gitignore`
* `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！
* https://github.com/github/gitignore 在线浏览各种配置文件

###配置别名
* `git config --global alias.st status`  :
	* 以后`st`就表示`status`, 
* `git config --global alias.unstage 'reset HEAD'` :
	* 这样修改后，当你敲入命令`git unstage test.py`, 实际上git执行的是`git reset HEAD test.py`
* 丧心病狂的`log`配置 :
	* `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

* 配置文件：
	* 配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
	* 每个仓库的Git配置文件都放在.git/config文件中，别名就在文件内容的[alias]后面，要删除别名，直接把对应的行删掉即可。
	* 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中：




