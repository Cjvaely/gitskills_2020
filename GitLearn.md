# Git教程

## 1 安装
在MacOS上，使用homebrew安装Git。到https://brew.sh/index_zh-cn安装homebrew。

```shell
brew install git
```

最后，在命令行上输入：

```shell
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

## 2 创建版本库(repository)

简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```shell
mkdir learngit
cd learngit
git init	// 变成git可以管理的仓库
```

![image-20200214154522210](/Users/cjv/Library/Application Support/typora-user-images/image-20200214154522210.png)

这时，多了一个目录.git：

![image-20200214154642939](/Users/cjv/Library/Application Support/typora-user-images/image-20200214154642939.png)

## 3 文件添加到版本库

版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

编写一个文件：readme.txt。内容如下：

```txt
Git is a version control system.
Git is free software.
```

+ 第一步，用`git add`告诉Git，把文件添加到仓库：

  执行上面的命令，没有任何显示。

+ 第二步，用`git commit`告诉Git，把文件提交到仓库：

  得到结果：![image-20200214155619074](/Users/cjv/Library/Application Support/typora-user-images/image-20200214155619074.png)

  `-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

  `git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

  `commit`可以一次提交很多文件，所以你可以多次`add`不同的文件。

## 4 时光机穿梭

继续修改readme.txt：

```txt
Git is a distributed version control system.
Git is free software.
```

运行`git status`命令看看结果：

![image-20200214160032626](/Users/cjv/Library/Application Support/typora-user-images/image-20200214160032626.png)

Git告诉我们`readme.txt`被修改了，可以用`git diff`这个命令看看具体修改的内容：

```shell
git diff readme.txt 
```

![image-20200214160225711](/Users/cjv/Library/Application Support/typora-user-images/image-20200214160225711.png)

`git diff`顾名思义就是查看difference，我们在第一行添加了一个`distributed`单词。

提交到仓库，和提交新文件是一样的：git add + git commit -m "add distributed"

总结：使用`git status`查看当前工作区状态；git diff查看修改的内容。

## 5 版本回退

每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。现在，我们回顾一下`readme.txt`文件一共有几个版本被提交到Git仓库里了：

版本1：wrote a readme file

```
Git is a version control system.
Git is free software.
```

版本2：add distributed

```
Git is a distributed version control system.
Git is free software.
```

版本3：append GPL

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：

![image-20200214161904119](/Users/cjv/Library/Application Support/typora-user-images/image-20200214161904119.png)

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

![image-20200214162001546](/Users/cjv/Library/Application Support/typora-user-images/image-20200214162001546.png)

启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在回退到上一个版本：

`git reset --hard HEAD^`

![image-20200214162503428](/Users/cjv/Library/Application Support/typora-user-images/image-20200214162503428.png)

```shell
cat readme.txt
Git is a distributed version control system.
Git is free software.
```

还可以继续回退。再用`git log`查看一下：

![image-20200214162745654](/Users/cjv/Library/Application Support/typora-user-images/image-20200214162745654.png)

最新的那个版本`append GPL`已经看不到了。如果想要回到最新版本，需要使用：

`git reset --hard f86fc`

![image-20200214163233047](/Users/cjv/Library/Application Support/typora-user-images/image-20200214163233047.png)

但是如果在终端找不到id，Git提供了一个命令`git reflog`用来记录你的每一次命令：

![image-20200214163500240](/Users/cjv/Library/Application Support/typora-user-images/image-20200214163500240.png)

**总结：**

+ HEAD指向版本就是当前的版本，使用`git reset --head commit_id`可以进行版本穿梭。
+ 穿梭前，可以使用`git log`查看提交历史，以便确定回退版本。
+ 重返未来，用`git reflog`查看历史命令。

## 6 工作区（Working Directory）和暂存区（Repository）

1. 工作区（Working Directory）

   就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区。

2. 版本库（Repository）

   隐藏目录`.git`，这个不算工作区，而是Git的版本库。

   ![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

   1. 用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
   2. 用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
   3. 创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

   > 需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

   

3. 然后测试一下：

   + 先对`readme.txt`做个修改，比如加上一行内容：

     ```
     Git is a distributed version control system.
     Git is free software distributed under the GPL.
     Git has a mutable index called stage.
     ```

   + 然后，在工作区新增一个`LICENSE`文本文件（内容随便写）：

   ![image-20200214164605908](/Users/cjv/Library/Application Support/typora-user-images/image-20200214164605908.png)

   ```
   git add readme.txt
   git add LICENSE
   ```

   ![image-20200214164729461](/Users/cjv/Library/Application Support/typora-user-images/image-20200214164729461.png)

   现在，暂存区的状态就变成这样了：

   ![git-stage](https://www.liaoxuefeng.com/files/attachments/919020074026336/0)

> 所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

提交之后，工作区就是干净的：

![git-stage-after-commit](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)





## 7 撤销修改

修改readme.txt文件后，如果要撤销修改：

![image-20200214170045446](/Users/cjv/Library/Application Support/typora-user-images/image-20200214170045446.png)

可以看到：`git checkout -- readme.txt`，就是把工作区上的修改全部撤销。两种情况：

1. readme.txt修改后没放到暂存区，撤销后就回到原来版本。
2. Readme.txt已添加到暂存区，又作了修改，现在撤销修改，回到添加到暂存区后的状态。
3. 总之，就是让文件回到最近一次git commit或git add。

如果提交了，git add：

![image-20200214171533867](/Users/cjv/Library/Application Support/typora-user-images/image-20200214171533867.png)

此时可以用`git reset HEAD readme.txt`，把暂存区的修改撤销，重新放回工作区。然后再使用`git checkout -- readme.txt`丢弃工作区的修改。

**小结：**

1. 改乱了工作区，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
2. 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD `，就回到了场景1，第二步按场景1操作。
3. 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。

## 8 删除文件

添加新文件test.txt，`git add test.txt`，`git commit -m "add test.txt"`。然后删除文件：`rm test.txt`。使用`git status`查看：

![image-20200214173435694](/Users/cjv/Library/Application Support/typora-user-images/image-20200214173435694.png)

现在有两个选择：

1. 确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`

   ```shell
   git rm test.txt
   git commit -m "remove test.txt"
   ```

2. 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

   ```shell
   git checkout -- test.txt
   ```

   `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。但是，注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

## 9 远程仓库

1. 创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

   ```
   $ ssh-keygen -t rsa -C "youremail@example.com"
   ```

   ![image-20200214174817344](/Users/cjv/Library/Application Support/typora-user-images/image-20200214174817344.png)

   可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

   ![image-20200214174937284](/Users/cjv/Library/Application Support/typora-user-images/image-20200214174937284.png)

   

2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：

   ![image-20200214175248445](/Users/cjv/Library/Application Support/typora-user-images/image-20200214175248445.png)

   然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

   ![image-20200214175441398](/Users/cjv/Library/Application Support/typora-user-images/image-20200214175441398.png)

## 10 添加远程库

已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

1. 登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：

   ![image-20200214180628303](/Users/cjv/Library/Application Support/typora-user-images/image-20200214180628303.png)

   目前，在GitHub上的这个仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

   ```shell
   git remote add origin https://github.com/Cjvaely/GitCode.git
   ```

   添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

2. 下一步，就可以把本地库的所有内容推送到远程库上：

   ```shell
   git push -u origin master
   ```

   ![image-20200214181038199](/Users/cjv/Library/Application Support/typora-user-images/image-20200214181038199.png)

3. 把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

   由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令`git push origin master`，把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

**总结：**

关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

## 11 从远程库克隆

假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills_2020`：

![image-20200214182252772](/Users/cjv/Library/Application Support/typora-user-images/image-20200214182252772.png)

我们勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件：

现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：

```shell
git clone git@github.com:michaelliao/gitskills.git
```





