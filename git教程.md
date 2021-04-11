

#### 为什么要用git

- 用git来进行版本控制，就不用在电脑里面保存很多个最终版、最最最终版这些文件了，不过要注意的是所有的版本控制系统（VCS version control system），其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，**图片、视频、word这些二进制文件可以用版本控制系统管理但是没办法知道具体的改动是什么**。
- 可以让用户协同对文件进行修改，不用把文件传来传去
- git使用了分布式版本控制系统（不必联网，安全性高）

#### Git 资源

On top of search results, here are some great Git resources available online:

- [Pro Git](https://git-scm.com/book/en/v2): This book (available online and in print) covers all the fundamentals of how Git works and how to use it. Refer to it if you want to learn more about the subjects that we cover throughout the course.
- [Git tutorial](https://git-scm.com/docs/gittutorial): This tutorial includes a very brief reference of all Git commands available. You can use it to quickly review the commands that you need to use.

#### git的下载（各个平台）

- Linux：

```
sudo apt-get install git
```

- MacOS

```
brew install git
```

或者下载Xcode

- Windows

  直接[下载](https://git-scm.com/download/win)默认安装

#### git的基本操作

##### 设置用户名和邮箱

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址

##### 创建版本库

- 通过`git init`命令把这个目录变成Git可以管理的仓库，你可以新建工作区，也可以在原有的工作区下进行操作

```
$ mkdir git_demo
$ cd git_demo
$ pwd
/c/Users/yunhu/Desktop/git_demo
```

![image-20200909090728555](git%E6%95%99%E7%A8%8B.assets/image-20200909090728555.png)

注意在我们git init之前git_demo这个文件夹下是没有任何的文件的，git init之后就出现了.git目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了

![image-20200909090955490](git%E6%95%99%E7%A8%8B.assets/image-20200909090955490.png)

##### 把文件添加到版本库

把文件添加到版本库需要git add和git commit两个命令

```
$ touch readme.txt
$ ls -ha
./  ../  .git/  readme.txt
$ git add readme.txt
$ git commit -m "add a readme file"
[master (root-commit) 8eff432] add a readme file
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 readme.txt
```

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件

![image-20200909091426366](git%E6%95%99%E7%A8%8B.assets/image-20200909091426366.png)

##### 查看仓库状态

- git status：这个命令可以让我们时刻掌握仓库当前的状态

- git diff ：这个命令告诉我们具体对什么进行了修改

  大家注意我在add之前，add之后commit之前和commit使用git status以及git diff的差异，后面讲工作区和版本库的时候大家就明白为什么是这样的了。

![image-20200909092433013](git%E6%95%99%E7%A8%8B.assets/image-20200909092433013.png)



##### 查看日志

```
git log
```

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```
$ git log --pretty=oneline
```

你看到的一大串类似`f41cf8`的是`commit id`（版本号）。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用简单的1，2，3……作为版本号，那肯定就冲突了。

每提交一个新版本，实际上Git就会把它们自动串成一条时间线。

![image-20200909092753627](git%E6%95%99%E7%A8%8B.assets/image-20200909092753627.png)

##### 版本切换

由于某些原因，我们需要把文件版本进行回退到 add a readme file的地方，那么首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`f41cf8`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上10000个版本写100个`^`比较容易数不过来，所以写成`HEAD~10000`。

现在，我们要把当前版本change for the first time回退到上一个版本add a readme file，

就可以使用`git reset`命令：

```
$ git reset --hard 8eff
HEAD is now at 8eff432 add a readme file
```

看看`readme.txt`的内容是不是版本First change：

![image-20200909093800169](git%E6%95%99%E7%A8%8B.assets/image-20200909093800169.png)

果然被还原了，我们用`git log`再看看现在版本库的状态：

![image-20200909094023023](git%E6%95%99%E7%A8%8B.assets/image-20200909094023023.png)

最新的那个版本change for the first time已经看不到了！那么我们怎么切换回change for the first time这个版本呢？

直接网上找找到change for the first time的commit id是 f41cf8....,直接用commit id 回到这个版本。

![image-20200909094404472](git%E6%95%99%E7%A8%8B.assets/image-20200909094404472.png)

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`add a readme file`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ change for the first time
        │
        ○ add a readme file
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ change for the first time
   │    │
   └──> ○ add a readme file
```

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本内容就是哪一个。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add a readme file`版本时，再想恢复到`change for the first time`，就必须找到`change for the first time`的`commit id`。Git提供了一个命令`git reflog`用来记录你的每一次命令：

![image-20200909100515688](git%E6%95%99%E7%A8%8B.assets/image-20200909100515688.png)

终于舒了口气，从输出可知，`change for the first time`的commit id是`f41cf81`，现在，你又可以乘坐时光机回到未来了。

##### 撤销修改

先直接放一下结论：

- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

  ![image-20200909105253834](git%E6%95%99%E7%A8%8B.assets/image-20200909105253834.png)

- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

  ![image-20200909105733342](git%E6%95%99%E7%A8%8B.assets/image-20200909105733342.png)

- 场景3：已经提交了不合适的修改到版本库时，这个时候相当于把工作区的内容提交到master分支了，想要撤销本次提交，直接看上面的版本切换，这里就不放实验了，不过前提是没有推送到远程库（后面也会讲到，提交到远程库之前大家谨慎就对了）。

##### 删除文件

删除文件主要分为两种情况：
1、你用rm删除了工作区中的文件，但是你后悔了，想要恢复，那就可以 `git checkout --<filename>` 

![image-20200909110830506](git%E6%95%99%E7%A8%8B.assets/image-20200909110830506.png)

2、你用rm删除了工作区中的文件，你确实想要删除这个文件，那就可以使用`git rm`命令

![image-20200909114201001](git%E6%95%99%E7%A8%8B.assets/image-20200909114201001-1600566898572.png)

这个时候你又后悔了怎么办：别忘了`git reset`

![image-20200909114243133](git%E6%95%99%E7%A8%8B.assets/image-20200909114243133-1600566900429.png)

但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**

#### 工作区和版本库

##### 工作区、缓存区、仓库

工作区就是你在电脑里能看到的目录。工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

- 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
- 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

还记得前面的查看仓库状态那一块我让大家注意add之后commit之前和commit使用git status以及git diff的差异吗？大家如果搞清楚了工作区，暂存区、仓库之间的关系就很好理解了，大家可以直接做实验试一下，我把结论先放在这：

- git add把文件从工作区>>>>暂存区，
- git commit把文件从暂存区>>>>仓库，
- git diff  查看工作区和暂存区差异，
- git diff --cached查看暂存区和仓库差异，
- git diff HEAD 查看工作区和仓库的差异，
- git add的反向命令git checkout，撤销工作区修改，即把暂存区最新版本转移到工作区，
- git commit的反向命令git reset HEAD，就是把仓库最新版本转移到暂存区。

##### git管理修改

有了前面的工作区、缓存区、仓库的概念之后，大家还需要了解的一个很重要的概念就是Git跟踪并管理的是修改，而非文件。

这里我们直接通过实验来看看，我们的实验大概是以下流程：
第二次修改 -> `git add` -> 第三次修改 -> `git commit`，然后查看一下HEAD指向的版本，看看HEAD指向的版本和工作区的差别
具体实现如下：

修改提交过程：

![image-20200909104111632](git%E6%95%99%E7%A8%8B.assets/image-20200909104111632.png)

查看状态：

![image-20200909104225364](git%E6%95%99%E7%A8%8B.assets/image-20200909104225364.png)

查看HEAD指向的版本和工作区的差别：

![image-20200909104238323](git%E6%95%99%E7%A8%8B.assets/image-20200909104238323.png)

我们会发现，第二次的修改没有被提交，其实也就是我们前面讲的每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中，所以Git其实每次都是在跟踪你的修改。

#### 远程仓库

到目前为止，我们其实相当于只体验的git的版本控制功能，对于git的分布式版本控制系统让用户能对文件进行协同式的处理这一功能我们还没有体验，我们一般会常用到两种方式去实现这一功能，一种是Github，这个相信大家应该多少都有点了解了，Github是一个提供公共Git仓库托管服务的平台，只要注册一个GitHub账号，就可以免费获得Git远程仓库，作为程序员可以说是必须了解的一个神奇网站，上面有很多的代码、教程等各种资源。另一个就是大家自己在服务器上搭建的Git远程管理仓库比如Gitlab，想很多公司不愿意把公司的代码放在Github上就会自己在服务器上搭建Gitlab等。

##### Github创建 SSH Keys

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以在这里我们要为Github创建SSH Keys，步骤如下：

***\*第一步：\****在用户主目录下,看有没有.ssh目录,如果有,再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件,如果已经有了,可直接跳到下一步,如果没有

打开Git Bash,创建SSH Key

```ruby
$ ssh-keygen -t rsa -C "youremail@xxx.com"
```

你需要把邮件地址换成你自己的邮件地址,然后一路回车,默认即可。

怎么找主目录直接看下面的图：

![image-20200909160042668](git%E6%95%99%E7%A8%8B.assets/image-20200909160042668-1600566931033.png)

如果一切顺利,可以在用户目录里找到.ssh目录 里面有id_rsa和id_rsa.pub两个文件

这两个就是SSH Key的秘钥,id_rsa是私钥,id_rsa.pub是公钥。

***\*第二步：\****登录GitHub 打开Settings,添加Key 如图

![image-20200909160438171](git%E6%95%99%E7%A8%8B.assets/image-20200909160438171-1600566933401.png)

成功之后,你就能看到添加的Key，还会收到邮件提醒你你的github上增加了key

![image-20200909160543097](git%E6%95%99%E7%A8%8B.assets/image-20200909160543097-1600566935534.png)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

##### 添加远程库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。

首先，登陆GitHub，然后，在右上角找到“Create a new repo（repository）”按钮，创建一个新的仓库：

![image-20200909161813627](git%E6%95%99%E7%A8%8B.assets/image-20200909161813627-1600566938305.png)

创建repo的时候，有一些设置，是什么作用我都放到这下面了：

![image-20200909163020385](git%E6%95%99%E7%A8%8B.assets/image-20200909163020385-1600566940468.png)

**切记**:如果我们在创建远程仓库的时候添加了README和.ignore等文件,我们在后面关联仓库后,需要先执行`pull`操作

目前，在GitHub上的这个`git_demo`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

![image-20200909164158923](git%E6%95%99%E7%A8%8B.assets/image-20200909164158923-1600566942864.png)

现在，我们根据GitHub的提示，在本地的`git_demo`仓库下运行命令：

```
$ git remote add origin https://github.com/Yunhui1998/git_demo.git
```

这里再教一个命令，就是删除与远程库的关联，比如说你想关联其他其他库的时候

```
$ git remote rm origin
```

请千万注意，把上面的`Yunhui1998`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

![image-20200909164743935](git%E6%95%99%E7%A8%8B.assets/image-20200909164743935-1600566945505.png)

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

在push的时候我们会发现，每次push都要属于用户名和密码，这实在是有点麻烦，因此教给大家一个[git push 免密码的方式单独放在下一节了。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![image-20200909170016514](git%E6%95%99%E7%A8%8B.assets/image-20200909170016514-1600566947722.png)

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub。

##### git push 免密码

有两种办法

**方法一：通用办法**

1.使用文件创建用户名和密码

文件创建在用户主目录下：

```shell
touch .git-credentials
vim .git-credentials
https://{username}:{password}@github.com
```

![image-20200909165740320](git%E6%95%99%E7%A8%8B.assets/image-20200909165740320-1600566954781.png)

**记得在真正输入的时候是没有大括号的。**

![image-20200909165859048](git%E6%95%99%E7%A8%8B.assets/image-20200909165859048-1600566952267.png)

2.添加git config内容

```
git config --global credential.helper store
```

执行此命令后，用户主目录下的.gitconfig文件会多了一项：`[credential]`

```
helper = store
```

重新git push就不需要用户名密码了。

**方法二：使用ssh协议**

首先生成密钥对：

```
ssh-keygen -t rsa -C "youremail"
```

接下来按照提示操作，默认可以一路往下。

然后将生成的位于`~/.ssh/`的`id_rsa.pub`的内容复制到你github setting里的ssh key中。

复制之后，如果你还没有克隆你的仓库，那你直接使用ssh协议用法：`git@github.com:yourusername/yourrepositoryname`克隆就行了。

如果已经使用https协议克隆了，那么按照如下方法更改协议：
`git remote set-url origin git@github.com:yourusername/yourrepositoryname.git`

##### 从远程库clone

clone其实就是把远程库复制/克隆（clone）到本地，有三种方式：

方法一：通过SSH命令，大概就是长这样：

```
git clone  git@github.com:Yunhui1998/git_demo.git
```

clone后面的这一串点开github上的绿色的Code就可以看得见了

![image-20200909171248645](git%E6%95%99%E7%A8%8B.assets/image-20200909171248645-1600566957834.png)

方法二：通过HTTPS命令，大概长这样：

```
git clone https://github.com/Yunhui1998/git_demo.git
```

clone后面的这一串点开github上的绿色的Code就可以看得见了

![image-20200909171642984](git%E6%95%99%E7%A8%8B.assets/image-20200909171642984-1600566959624.png)

方法三：直接下载ZIP文件

![image-20200909171756320](git%E6%95%99%E7%A8%8B.assets/image-20200909171756320-1600566961487.png)

方法三就没什么好讲的了，我们来分析一下用https和ssh命令的差别：

- https用443端口，可以对repo根据权限进行读写，只要有账号密码就可进行操作。
- ssh则用的是22端口，也可以对repo根据权限进行读写，但是需要SSH Keys授权，这个key是通过ssh key生成器生成的，然后放在github上，作为授权的证据，这样的话就不需要用户名和密码进行授权了。

##### git pull和git clone的区别

一、git pull命令用于取回远程主机某个分支的更新与本地的指定分支合并。

二、git clone是把整个git项目拷贝下来，包括里面的日志信息，git项目里的分支，你也可以直接切换、使用里面的分支等等



##### 新建远程分支

新建一个本地分支：

```ruby
$ git checkout -b dbg_lichen_star
```

查看一下现在的分支状态:

```ruby
$ git branch
* dbg_lichen_star
  master
  release
```

星号(*)表示当前所在分支。现在的状态是成功创建的新的分支并且已经切换到新分支上。

把新建的本地分支push到远程服务器，远程分支与本地分支同名（当然可以随意起名）：

```ruby
$ git push origin dbg_lichen_star:dbg_lichen_star
```

使用`git branch -a`查看所有分支，会看到`remotes/origin/dbg_lichen_star`这个远程分支，说明新建远程分支成功

##### 删除远程分支

我比较喜欢的简单方式，推送一个空分支到远程分支，其实就相当于删除远程分支：

```ruby
$ git push origin :dbg_lichen_star
```

也可以使用：

```
git push origin --delete dbg_lichen_star
```

##### git 换行符问题					 				 				 				 			

GNU/Linux和Mac OS使用换行(`LF`)或新行作为行结束字符，而Windows使用换行和回车(`LFCR`)组合来表示行结束字符。

为了避免这些行结尾的差异的不必要提交，我们必须配置Git客户端写入与Git仓库使用相同的行结束符。

对于Windows系统，可以将Git客户端配置为将行结束符转换为`CRLF`格式，同时退出，并在提交操作时将其转换回`LF`格式。以下可根据您的需要来设置。

```shell
$ git config --global core.autocrlf true
Shell
```

对于*GNU/Linux*或*Mac OS*，我们可以配置Git客户端，以便在执行结帐操作时将线结束从**CRLF**转换为**LF**。

```shell
yiibai@ubuntu:~$  git config --global core.autocrlf input
```



#### 分支管理

##### 创建分支与合并

如果说git只有master一个分支的话，那么很多人都往上面去push自己的内容的时候其实是会让master特别不稳定的，而且比如说你开发一个新功能代码只写了一半，直接push到远程的话就会导致其他在用master分支的人就用不了。这个时候就需要去创建分支了，你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。而且对于git来说，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

我们直接通过例子来看看是怎么建立分支的。

首先，我们创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

```
we change git for the third time
```

然后提交：

```
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```
$ git switch master
Switched to branch 'master'
```

切换回`master`分支后，再查看一个`readme.txt`文件，

```
yunhui@DESKTOP-LKM7QCV MINGW64 ~/Desktop/git_demo (master)
$ cat readme.txt
We change git for the first time
we change git for the second time
```

刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](git%E6%95%99%E7%A8%8B.assets/0-1600566966142)

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```
$ yunhui@DESKTOP-LKM7QCV MINGW64 ~/Desktop/git_demo (master)
$ git merge dev
Updating 9cee7e1..8dac1ee
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)

yunhui@DESKTOP-LKM7QCV MINGW64 ~/Desktop/git_demo (master)
$ cat readme.txt
We change git for the first time
we change git for the second time
we change git for the third time
merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

删除后，查看`branch`，就只剩下`master`分支了：

```
$ git branch
* master
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

这里总结一下对分支的操作：

```
管理分支
grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   merge      Join two or more development histories together
                                                                   -------------裁切自命令行git --help

//创建 <name> 的分支
git branch <name>

//切换到<name>分支
git checkout <name>
git switch <name>

//查看分支树
git branch 

//删除分支
git branch -d <name>

//合并分支 假设这里存在 master ， dev 分支
  // 1 切换到要保留的分支——这里是master
  git checkout master
  // 2 合并掉分支dev
  git merge dev
```

##### 解决冲突

在对分支进行合并的时候，很有可能是会发生冲突的，比如说我们先在dev分支上的readme.txt文件上面加上一行，然后我们把这个改动提交到dev分支，之后我们在master的readme分支上也加上一行，然后提交到master分支，这个时候如果我们要将dev快速合并到master分支上时就会出现问题了，也就是发生了冲突。我们通过实验来看一下：

![image-20200909203044432](git%E6%95%99%E7%A8%8B.assets/image-20200909203044432-1600566969023.png)

这里提示我们出现了冲突，我们直接看一下readme.txt的内容，Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容。

![image-20200909203440329](git%E6%95%99%E7%A8%8B.assets/image-20200909203440329-1600566971158.png)

我们修改如下后保存然后再提交就好了。

![image-20200909203934176](git%E6%95%99%E7%A8%8B.assets/image-20200909203934176-1600566973066.png)

我们用`git log --graph --pretty=oneline --abbrev-commit`可以看一下分支合并的过程：

![image-20200909204234159](git%E6%95%99%E7%A8%8B.assets/image-20200909204234159-1600566975120.png)

##### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](git%E6%95%99%E7%A8%8B.assets/0)

##### Bug分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

**幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作**：

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

**一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；**

**另一种方式是用`git stash pop`，恢复的同时把stash内容也删了**：

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何stash内容了：

```
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```

在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

那怎么在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

小结：

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

##### Feature 分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

现在，你终于接到了一个新任务：开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船。

于是准备开发：

```
$ git switch -c feature-vulcan
Switched to a new branch 'feature-vulcan'
```

5分钟后，开发完毕：

```
$ git add vulcan.py

$ git status
On branch feature-vulcan
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   vulcan.py

$ git commit -m "add feature vulcan"
[feature-vulcan 287773e] add feature vulcan
 1 file changed, 2 insertions(+)
 create mode 100644 vulcan.py
```

切回`dev`，准备合并：

```
$ git switch dev
```

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

但是！

就在此时，接到上级命令，因经费不足，新功能必须取消！

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

```
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。。

现在我们强行删除：

```
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

终于删除成功！

##### git中的多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```
$ git remote
origin
```

**推送分支**

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

**抓取分支**

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。比如说在项目当中有人对仿真环境进行了更新，你要开始用新的环境去做实验了，那么你就需要把别人的更新pull（抓取）下来。

```
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```

比如，要取回`origin`主机的`next`分支，与本地的`master`分支合并，需要写成下面这样 -

```shell
$ git pull origin next:master
```

##### 实际项目中用git做分支管理

下面的内容主要来自于：https://nvie.com/posts/a-successful-git-branching-model/

在实际开发中如何使用`Git`没有一个标准答案，使用方式也是各式各样，很多基本上都是把Git当SVN来用。下面介绍的是一种经过实践的运行比较良好的管理方式。

**主分支**：

实际开发中，一个仓库（通常只放一个项目）主要存在两条主分支：**master**与**develop**分支。这个两个分支的生命周期是整个项目周期。就是说，自创建出来就不会删除，会随着项目的不断开发不断的往里面添加代码。master分支是创建git仓库时自动生成的，随即我们就会从master分支创建develop分支，如下图所示。
![这里写图片描述](git%E6%95%99%E7%A8%8B.assets/20180624162549140-1600566985283)

- **master**：这个分支最为稳定，这个分支代表项目处于可发布的状态。

  例如臭皮匠向`master`分支合并了代码，那就意味着臭皮匠完成了此项目的一个待发布的版本，项目经理可以认为，此项目已经准备好发布新版本了。所以`master`分支不是随随便便就可以签入代码的地方，只有计划发布的版本功能在`develop`分支上全部完成，而且测试没有问题了才会合并到`master`上。

- **develop**：作为开发的分支，平行于master分支。

  例如臭皮匠要开发一个**注册功能**，那么他就会从`develop`分支上创建一个**feature**分支 `fb-register`（后面讲），在`fb-register`分支上将注册功能完成后，将代码合并到**develop**分支上。这个`fb-register`就完成了它的使命，可以删除了。项目经理看臭皮匠效率很高啊，于是：“臭皮匠你顺带把登录功能也做了吧”。臭皮匠就会重复上面的步骤：从develop分支上新创建一个名为`fb-login`的分支，喝杯咖啡继续开发，1个小时后登录功能写好了，臭皮匠又会将这个分支的代码合并回**develop**分支后将其删除。

**支持分支**：

这些分支都是为了程序员协同开发，以及应对项目的各种需求而存在的。这些分支都是为了解决某一个具体的问题而设立，当这个问题解决后，代码会合并回主分支**develop**或者**master**后删除，一般我们会人为分出三种分支。

- **Feature branches**：这种分支和我们程序员日常开发最为密切，称作功能分支。

  必须从**develop**分支创建，完成后合并回**develop**分支

  如下图所示。
  ![img](git%E6%95%99%E7%A8%8B.assets/main-branches@2x.png)

  具体事例可以参考上面王二狗完成登录注册功能时的做法。

- **Release branches**：这个分支用来发布新版本。

  从**develop**分支创建，完成后合并回**develop**与**master**分支。

  这个分支上可以做一些非常小的bug修复，当然，你也可以禁止在这个分支做任何bug的修复工作，而只做版本发布的相关操作，例如设置版本号等操作，那样的话那些发现的小bug就必须放到下一个版本修复了。如果在这个分支上发现了大bug，那么也绝对不能在这个分支上改，需要**Featrue**分支上改，走正常的流程。

  **实例**：臭皮匠开发完了登录注册功能后决定发一个版本V0.1，那么他先从**develop**分支上创建一个**Release** 分支`release-v0.1`，然后臭皮匠在这个分支上把版本号等做了修改。正准备编译发布了，项目经理说：“臭皮匠啊，你那个登录框好像向右偏移量**1个像素**，你可以调一下吗？”。臭皮匠发现这个bug特别小，对项目其他部分不造成不可预知的问题，所以直接在release分支上改了，然后愉快的发布了版本1.0.版本上线后，臭皮匠将这个分支分别合并回了**develop**与**master**分支，然后删除了这个分支。

- **Hotfix branches**：这个分支主要为修复线上特别紧急的bug准备的。

  必须从**master**分支创建，完成后合并回**develop**与**master**分支。

  如下图所示：
  ![img](git%E6%95%99%E7%A8%8B.assets/hotfix-branches@2x.png)
  这个分支主要是解决线上版本的紧急bug修复的，例如突然版本V0.1上有一个致命bug，必须修复。那么我们就可以从**master** 分支上发布这个版本那个时间点 例如 **tag v0.1**（一般代码发布后会及时在master上打**tag**），来创建一个 `hotfix-v0.1.1`的分支，然后在这个分支上改bug，然后发布新的版本。最后将代码合并回**develop**与**master**分支。

**实例**：某天夜项目经理打来电话：“臭皮匠啊，线上出了个大问题，大量用户无法登录，客服电话已经被打爆了，你紧急处理一下”。臭皮匠先找到**master**分支上**tag v0.1** 的地方，然后新建了`hotfix-v0.1.1` 分支，默默的修bug去了。经过一个多小时的奋战，终于修复了，然后臭皮匠改了版本号，发布了新版本。臭皮匠将这个分支分别合并回了**develop**与**master**分支后删除了这个分支。

**总结图**：

上面的讲解最后汇成一张图
![img](git%E6%95%99%E7%A8%8B.assets/git-model@2x.png)

#### git 命令速查

##### git help

- 作用：`git help`命令显示有关Git的帮助信息

- 模板：

  ```
  git help [-a|--all] [-g|--guide]         [-i|--info|-m|--man|-w|--web] [COMMAND|GUIDE]
  ```

  如果给出`--all`或`-a`选项，则所有可用的命令都将打印在标准输出上。

  如果给出了-`-guide`或`-g`选项，那么在标准输出中也会列出有用的Git指南。

  如果你直接git help 某个命令的话是会跳出网页而不是直接打在控制台上的。

- 举例：

  比如，想要查看 `push`命令如何使用，可以使用以下命令 - 

  ```shell
  $ git help push
  ```



##### git config

- 作用：`git config`命令用于获取并设置存储库或全局选项。这些变量可以控制Git的外观和操作的各个方面

- 模板：

  ```
  git config [<file-option>] [type] [--show-origin] [-z|--null] name [value [value_regex]]
  git config [<file-option>] [type] --add name value
  git config [<file-option>] [type] --replace-all name value [value_regex]
  git config [<file-option>] [type] [--show-origin] [-z|--null] --get name [value_regex]
  git config [<file-option>] [type] [--show-origin] [-z|--null] --get-all name [value_regex]
  git config [<file-option>] [type] [--show-origin] [-z|--null] [--name-only] --get-regexp name_regex [value_regex]
  git config [<file-option>] [type] [-z|--null] --get-urlmatch name URL
  git config [<file-option>] --unset name [value_regex]
  git config [<file-option>] --unset-all name [value_regex]
  git config [<file-option>] --rename-section old_name new_name
  git config [<file-option>] --remove-section name
  git config [<file-option>] [--show-origin] [-z|--null] [--name-only] -l | --list
  git config [<file-option>] --get-color name [default]
  git config [<file-option>] --get-colorbool name [stdout-is-tty]
  git config [<file-option>] -e | --edit
  ```

  git config能做的东西太多了，建议大家每次要改啥直接去查就好了，直接给几个常用的例子给大家

- 举例：

  - 配置用户名和密码

  当安装Git后首先要做的事情是设置用户名称和e-mail地址。这是非常重要的，因为每次Git提交都会使用该信息。它被永远的嵌入到了你的提交中：

  ```shell
  $ git config --global user.name "maxsu"
  $ git config --global user.email "yiibai.com@gmail.com"
  ```

  -    配置编缉器

    可以配置默认的文本编辑器，Git在需要你输入一些消息时会使用该文本编辑器。缺省情况下，Git使用系统的缺省编辑器，这通常可能是 `vi` 或者 `vim` 。如果想使用一个不同的文本编辑器，例如：`Emacs`，可以按照如下操作：

    ```shell
    $ git config --global core.editor emacs
    ```



##### git init

- 作用：it init`命令创建一个空的Git仓库或重新初始化一个现有仓库。

- 模板：

  ```shell
  git init [-q | --quiet] [--bare] [--template=<template_directory>]
        [--separate-git-dir <git dir>]
        [--shared[=<permissions>]] [directory]
  ```

  其实上面的这些参数说实话都不太常用，最常用的就是直接git init，也就是为现在的工作区启动一个新的仓库

- 举例：

  为现有的代码库启动一个新的Git仓库

  ```
  git init
  ```



##### git add

- 作用

  `git add`命令将文件内容添加到索引(将修改添加到暂存区)。也就是将要提交的文件的信息添加到索引库中。

- 模板

  ```
  git add [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
        [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]]
        [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing]
        [--chmod=(+|-)x] [--] [<pathspec>…​]
  ```

- 举例

  - 添加`documentation`目录及其子目录下所有`*.txt`文件的内容：

    ```
    git add documentation/*.txt
    ```

  - 将所有修改添加至暂存区

    ```
    git add .
    ```

  - 添加某一个文件或者文件夹

    ```
    git add <path>
    ```

    `<path>`可以是文件也可以是目录。



##### git commit

- 作用

  `git commit`命令用于将更改记录(提交)到存储库。将索引的当前内容与描述更改的用户和日志消息一起存储在新的提交中。

- 模板

  ```
  git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
         [--dry-run] [(-c | -C | --fixup | --squash) <commit>]
         [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
         [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
         [--date=<date>] [--cleanup=<mode>] [--[no-]status]
         [-i | -o] [-S[<keyid>]] [--] [<file>…​]
  ```

  注意git commit其实是把暂存区的所有内容都提交到了仓库，所以你可以多次add之后进行一次commit。

- 举例

  在提交的时候你可以就用`git commit`,但是我们用的更多的是

  ```
  git commit -m "你本次提交的改动"
  ```

  就相当于你每次提交的时候写一个简单的注释，这样对你后期进行版本切换等都会产生帮助。



##### git status

- 作用：`git status`命令用于显示工作目录和暂存区的状态。使用此命令能看到那些修改被暂存到了, 哪些没有, 哪些文件没有被Git tracked到。`git status`不显示已经`commit`到项目历史中去的信息。

- 模板

  ```
  git status [<options>…​] [--] [<pathspec>…​]
  ```

- 举例

  大家一般都直接用`git status`用的比较多，比如说你add文件到暂存区之后，你还没有提交，这个时候用`git status`就会提示你已经add但是没有commit。

  ```
  git status
  ```



##### git log

- 作用：`git log`命令用于显示提交日志信息。

- 模板：

  ```
  git log [<options>] [<revision range>] [[\--] <path>…]
  ```

- 举例

  我一般用git log更多的是来查看版本号，也经常会对输出的结果进行一些限定，比如说

  ```
  $ git log --pretty=oneline
  ```

  ![image-20200909092753627](git%E6%95%99%E7%A8%8B.assets/image-20200909092753627.png)

##### git diff

- 作用：`git diff`命令用于显示提交和工作树等之间的更改。此命令比较的是工作目录中当前文件和暂存区域快照之间的差异,也就是修改之后还没有暂存起来的变化内容。 

- 模板：

  ```
  git diff [options] [<commit>] [--] [<path>…​]
  git diff [options] --cached [<commit>] [--] [<path>…​]
  git diff [options] <commit> <commit> [--] [<path>…​]
  git diff [options] <blob> <blob>
  git diff [options] [--no-index] [--] <path> <path>
  ```

- 举例:

  ```
  git diff <file> # 比较当前文件和暂存区文件差异 git diff
  
  git diff <id1><id1><id2> # 比较两次提交之间的差异
  
  git diff <branch1> <branch2> # 在两个分支之间比较
  git diff --staged # 比较暂存区和版本库差异
  
  git diff --cached # 比较暂存区和版本库差异
  
  git diff --stat # 仅仅比较统计信息
  ```

##### git reset

- 作用:`git reset`命令用于将当前`HEAD`复位到指定状态。一般用于撤消之前的一些操作(如：`git add`,`git commit`等)。

- 模板:

  ```
  git reset [-q] [<tree-ish>] [--] <paths>…​
  git reset (--patch | -p) [<tree-ish>] [--] [<paths>…​]
  git reset [--soft | --mixed [-N] | --hard | --merge | --keep] [-q] [<commit>]
  ```

  `mode`的取值可以是`hard`、`soft`、`mixed`、`merged`、`keep`。

  目前我用的比较多的就是`hard`。`--hard`：重设(reset) 索引和工作目录，自从`<commit>`以来在工作目录中的任何改变都被丢弃，并把HEAD指向`<commit>`。

- 举例:

  - 回到上一个版本

  ```
  git reset --hard HEAD^   
  ```

  - 回到上上一个版本

  ```
  git reset --hard HEAD^^
  ```

  - 回到指定版本

  ```
  git reset --hard 版本号
  ```

##### git rm

- 作用:`git rm`命令用于从工作区和索引中删除文件。

- 模板：

  ```
  git rm [-f | --force] [-n] [-r] [--cached] [--ignore-unmatch] [--quiet] [--] <file>…
  ```

  这里后面的变量其实和linux删除文件一样，`-f`表示强制删除。`-r`可以用来删除文件夹下面的所有文件。

- 举例

  - 删除`text1.txt`文件，并把它从git的仓库管理系统中移除

  ```
  git rm text1.txt
  ```

  - 删除文件夹：`mydir`，并把它从git的仓库管理系统中移除。

  ```
  git rm -r mydir
  ```

##### git mv

- 作用：`git mv`命令用于移动或重命名文件，目录或符号链接。
- 模板：

```
git mv [-v] [-f] [-n] [-k] <source> <destination>
git mv [-v] [-f] [-n] [-k] <source> ... <destination directory>
```

在第一种形式中，它将重命名`<source>`为`<destination>`，`<source>`必须存在，并且是文件，符号链接或目录。 在第二种形式中，最后一个参数必须是现有的目录; 给定的源(`<source>`)将被移动到这个目录中。

索引在成功完成后更新，但仍必须提交更改。

- 举例：

  把一个文件：*text.txt* 移动到 *mydir*，可以执行以下操作 - 

  ```
  $ git mv text.txt mydir
  ```

  运行上面的 `git mv` 其实就相当于运行了`3`条命令：

  ```
  $ mv test.txt mydir/
  $ git rm test.txt
  $ git add mydir
  ```

##### git branch

- 作用：`git branch`命令用于列出，创建或删除分支。

- 模板：

  ```
  git branch [--color[=<when>] | --no-color] [-r | -a]
      [--list] [-v [--abbrev=<length> | --no-abbrev]]
      [--column[=<options>] | --no-column] [--sort=<key>]
      [(--merged | --no-merged) [<commit>]]
      [--contains [<commit]] [--no-contains [<commit>]]
      [--points-at <object>] [--format=<format>] [<pattern>…​]
  git branch [--set-upstream | --track | --no-track] [-l] [-f] <branchname> [<start-point>]
  git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]
  git branch --unset-upstream [<branchname>]
  git branch (-m | -M) [<oldbranch>] <newbranch>
  git branch (-d | -D) [-r] <branchname>…
  git branch --edit-description [<branchname>]
  ```

   选项`-r`导致远程跟踪分支被列出，而选项`-a`显示本地和远程分支。

- 举例：

  - 查看当前有哪些分支：

    ```
    $ git branch
      master
    * zyh
    ```

    上面结果显示，在本地有两个分支：master和zyh，*表示当前分支是zyh。

  - 查看远程分支：

    ```
    git branch -a
    ```

  - 新建一个分支choupijiang

    ```
    git branch choupijiang
    ```

  - 切换到分支zyh

    ```
    git checkout zyh
    git switch zyh  
    ```

    switch命令在版本低的时候可能用不了。

  - 将分支zyh名字改成choupijiang

    ```
    git branch -m zyh choupijiang
    ```

  - 删除远程分支zyh

    ```
    git push origin --delete zyh
    ```

  - 删除本地分支zyh

    ```
    git branch -d zyh
    ```

  - 合并分支zyh到分支master

    ```
    git merge zyh:master
    ```

    如果是合并到当前分支就直接`git merge zyh`

##### git checkout

- 作用
- 模板
- 举例

##### git merge

##### git stash

##### git tag

##### git pull

##### git push

##### git fetch

##### git remote

##### git rebase







