[TOC]

## 常用命令

### git clone

含义：从GitHub等代码托管平台克隆别人的项目

用法：`git clone [url]`

步骤：

1. 打开GitHub并找到你需要的项目

2. 点击`Clone or Download`，选择`Clone with SSH`或者`Clone with HTTPS`

   如图：![gitClone](./images/gitClone.png)

3. 在git-bash中输入如下命令：`git clone git@github.com:dujinyu/faker.js.git`即可克隆该项目到本地。

4. 当然你也可以选择`Use HTTPS`的方式，或者`Download ZIP`,看你想用那种方式。

### git init

含义：将一个文件夹初始化为一个Git仓库

用法：`git init [project-name]`

用法详解：

​	**法一**、你可以在你想要的地方，创建一个工作文件夹或者已存在的文件夹，然后进入到该文件夹，执行命令`git init`，既初始化完成。初始化完成后，会在该文件夹中生成一个名为`.git`的隐藏文件，看着就好，一定一定一定不要动它。

​	**法二**、直接执行命令`git init myProject`，这样会创建名字叫`myProject`的文件夹，并将其初始化。初始化完成后，会在该文件夹中生成一个名为`.git`的隐藏文件，看着就好，一定一定一定不要动它。

如图：

初始化如下：

![git init](./images/gitInit.png)

隐藏文件如下：

![.git](./images/.git.png)

### git status

含义： 查看**工作区**和**暂存区**当前状态

用法： `git status`

图示：

![git status](./images/gitStatus.png) 

红色部分是存在于**工作区**的内容，绿色部分是存在于**暂存区**的内容。

### git add

含义：将工作区的内容放到暂存区

用法：`git add [filename]`

用法详解：

​	如**git status**中的图示，我们想要将`2.txt`从工作区加入到暂存区，可以执行命令`git add 2.txt`，如果工作区有很多文件有待加入暂存区，我么为了一次操作全部加入，则可以执行命令`git add .`，有一个**点**。

### git commit

含义：将**暂存区**中的内容放入**仓库区**

用法：`git commit -m 'descriptive message'`

用法详解：`descriptive message`是对你将要加入仓库区的内容的一个描述，描述你这里面都做了哪些改动。需要用**双引号或单引号**引起来。`-m`是`--message`的意思，二者一致。如果不添加该参数，则会在`commit`执行是跳到`vi`编辑器中添加。`vi`编辑器请另行百度。

### git reset HEAD

含义：将暂存区的内容取消暂存，退回到工作区。

用法：`git reset HEAD filename`

用法详解：如下图，文件`2.txt`已经被加入到暂存区，我们可以执行`git reset HEAD 2.txt `来取消暂存`unstage`，该文件重新回到工作区。

![git reset HEAD](./images/gitResetHEAD.png)

取消暂存后的效果图：

![git reset HEAD 2.txt](./images/gitResetHEAD-1.png)