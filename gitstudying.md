# Git教程

## [指导](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

## Git简介

版本库（仓库）repository

#### 创建版本库

初始化一个Git仓库，使用 git init 命令

#### 添加文件到Git仓库

- 使用命令 `git add <file> `，可反复多次使用，添加多个文件；

- 使用命令 `git commit -m<message>`完成。

- 使用 `git status` 可以随时掌握工作区的状态

- 如果`git status` 告诉有文件被修改了，可以通过git diff查看修改内容

## 时光穿梭机

#### 版本回退

- `HEAD` 指向当前版本，因此可以通过`git reset -hard commitn-id`在版本的历史之间穿梭
- 穿梭前可以通过`git log` 查看历史
- 要重返未来，用 `git reflog`查看命令历史

#### 工作区和暂存区

##### 工作区

就是你在电脑里能看到的目录，比如`learngit`文件夹就是一个工作区

##### 版本库

工作区的一个隐藏目录`.git`为git的版文库

![image-20200413173201627](E:\Typora\学习笔记\image-20200413173201627.png)

`git add`实际把文件修改添加到暂存区

![image-20200413174744862](E:\Typora\学习笔记\image-20200413174744862.png)

`git commit`实际是把暂存区的所有内容提交到当前分支，一旦提交，暂存区就没有任何内容了

![image-20200413174919774](E:\Typora\学习笔记\image-20200413174919774.png)



#### 管理修改

#### 撤销修改

- 场景一：当你在工作区中乱改了某修东西，想要直接丢弃工作区的修改时，用`git checkout -- <file>`。
- 场景二：当你不但乱改了还添加到了暂存区，想要丢弃修改，先用`git reset HEAD <file>`,回到场景一，再操作场景一
- 场景三：当你不仅添加到暂存区还提交了，想要撤回本次提交，可以通过**版本回退**

#### 删除文件

命令`git rm <file>`或者`rm <file>`用于删除一个文件。如果一个文件已经被提交到版本库。则可以用`git checkout -- <file>`把版本库的文件版本替换到工作区，即只能恢复文件到最新提交版本，但是你会丢失最近一次后再修改而未提交的内容。

## 远程仓库

实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交

##### 设置密钥

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

`$ ssh-keygen -t rsa -C "youremail@example.com"`

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![image-20200413225340980](E:\Typora\学习笔记\image-20200413225340980.png)

GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

#### 添加远程库

- 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`

  例如` git remote add origin git@github.com:1648958661/Garbage.git`这样

- 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；此后，每次提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

- 分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

#### 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但`ssh`协议速度最快。