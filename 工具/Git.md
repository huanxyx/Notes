##Git
####基本操作
- 1.设置全局配置
	- `git config --global user.name "name"`
	- `git config --global user.email "email"`
- 2.创建版本库
	- 创建一个文件夹
	- `git init`
- 3.将文件添加到Git仓库中
	- `git add 文件名`:添加到仓库
	- `git commit -m "本次提交的说明"`:提交到仓库
- 4.查看当前仓库的状态以及不同
	- `git status`
	- `git diff`
- 5.查看历史记录以及版本回退
	- `git log`
		- 加上`--pretty=oneline`可以避免输出信息太多
		- `44f8160c8e6b692a35f88c4e38c48be0f59c313c`是版本号
	- `git reset`
		- `git reset --hard HEAD^`:回退到上一个版本
		- `HEAD^^`:上上个版本
		- `HEAD~100`:回退到往上100个版本
		- `git reset --hard 版本号`:回退到指定版本(版本号没必要写全,会自己去寻找)
	- `git reflog`
		- 记录每一次执行的命令
- 6.撤销工作区的修改,删除文件
	- `git checkout -- 文件名`
		- 文件还未放到暂存区,撤销后回到版本库一样的状态
		- 文件已经添加到暂存区,后做了修改,撤销后回到暂存区后的状态
		- 总之,回到最近`git commit`或`git add`时的状态
	- `git rm`

####远程仓库(GitHub)
- 1.SSH Key设置
	- 原因:本地Git仓库和GitHub仓库之间的传输是通过SSH加密的,GitHub需要识别出你推送的提交确实是你推送的.
	- 步骤:
		- 1.`ssh-keygen -t rsa -C "邮箱"`:创建SSHKey,在用户主目录下的`.ssh目录`的`id_rsa,id_rsa.pub`
		- 2.登录GitHub,打开个人设置,将SSH keys添加进去(id_rsa.pub)
- 2.远程仓库
	- `git remote add origin git@server-name:path/repo-name.git`
		- 关联远程仓库
		- `origin`是远程库的名字
	- `git push -u origin branch-name`
		- 将本地库的内容推送到远程
		- 第一次使用加了`-u`参数:将本地的`master`分支与远程的`master`分支关联起来,在以后的推送或者拉取时就可以简化命令(不需要加`-u`).
		- 第一次推送时会出现SSH警告,该警告需要确定GitHub的Key指纹信息,是否真来自GitHub服务器.
	- `git pull 远程主机名 远程分支[:本地分支名]`
		- 取回远程主机的某个分支,再与本地指定分支合并.(默认当前分支)
		- `git pull`:从远程分支更新当前分支
- 3.克隆
	- `git clone git@server-name:path/repo-name.git`
- 4.查看远程仓库信息
	- `git remote -v`
- 5.建立本地分支和远程分支的关联
	- `git branch --set-upstream-to=origin/dev dev`
- 6.在本地创建和远程分支对应的分支
	- `git checkout -b branch-name origin/branch-name`

####分支管理
- 1.创建,查看,合并,删除分支
	- `git branch <name>`
		- 创建分支
	- `git checkout <name>`
		- 切换分支
	- `git checkout -b <name>`
		- 创建并切换分支
    - `git branch`
    	- 查看当前分支
    - `git merge <name>`
    	- 合并分支
    	- 加上`--no-ff`使用普通模式合并
    - `git branch -d <name>`
    	- 删除分支(已合并)
    - `git branch -D <name>`
    	- 强制删除
- 存储工作现场
	- `git stash`
		- 将当前工作现场"存储起来"
	- `git stash list`
		- 查看存储的工作现场
	- `git stash apply`
		- 恢复工作现场
	- `git stash drop`
		- 删除工作现场
	- `git stash pop`
		- 恢复的同时删除

####标签管理
- `git tag <name>`
	- 创建标签
- `git tag <name> <历史版本id>`
	- 给历史版本添加标签
- `git tag`
	- 查看所有标签
- `git show <tagName>`
	- 查看标签信息
- `git tag -a <tagName> -m "描述"`
	- `-a`指定标签名
	- `-m`指定说明文字
- `git tag -d <name>`
	- 删除标签
- `git push orgin <tagName>`
	- 推送标签到远程
- `git push origin :refs/tags/<tagName>`
	- 从远程删除标签

####自定义git
- [忽略特殊文件](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758404317281e54b6f5375640abbb11e67be4cd49e0000)
- [配置别名](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375234012342f90be1fc4d81446c967bbdc19e7c03d3000)
- [搭配Git服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)

##Git概念
- 1.工作区(Working Directory)
	- 能看见的目录
- 2.版本库(Repository)
	- 工作区中的隐藏目录.git
	- 组成部分
		- stage暂存区
			- `git add`把文件修改添加到暂存区中.
		- 分支
			- `git commit` 把暂存区的所有内容提交到当前分支
		- 指向当前分支的指针HEAD
- 3.分支冲突  
	- 两个分支同时修改了,合并的时候就会出现冲突
- 4.快速合并和普通合并:
	- 快速合并是指将当前的分支指针直接移动到目标分支上
	- 普通合并是指将当前的分支通过目标分支的合并到一个新的状态

##扩展
- 所有版本控制系统只能跟踪文本文件的改动
- Git比其他版本控制系统设计得优秀的原因是:Git跟踪并管理的是修改,而非文件.
- `pull`到远程仓库时出现`更新被拒绝，因为远程仓库包含您本地尚不存在的提交。这通常是因为另外
`的错误,解决方案:
	- 强行上传:`git push -u origin +master`
	- 尽量先同步github上的代码到本地,在上面更改之后再上传
- Git支持多种协议:
	- 默认的`git://`使用ssh
	- 也可以使用`https://`
- 在github上克隆别人的仓库时,需要先克隆到自己的github仓库中,在克隆到本地电脑