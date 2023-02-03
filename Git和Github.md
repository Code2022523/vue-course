## git简介

项目的开发是一个不断迭代的过程，开发过程中程序员需要不断的对代码进行编写和更正。这就
带来很多的问题。首先，开发中代码会存在多个版本，我们如何将代码在多个版本间进行切换？
第二，代码上线后，如何在不影响现行开发工作的情况下对代码进行维护？第三，开发时某段代
码被多人修改时，如何处理代码的冲突问题？除此之外，还有存储效率、远程仓库等问题。
git是一个免费开源的版本控制系统，它被设计用来快速高效地管理项目开发的源码。通过git可
以跟踪代码的状态，也可以在修改代码后对代码状态进行存储，还可以在需要时将已经修改过的
代码恢复到之前存储的状态。更强大的是使用git管理代码时，可以创建代码分支（branch)，
代码分支相当于一段独立的代码记录，我们可以在分支上对代码进行任意的修改，而这个修改只
会影响当前分支，不会对其他分支产生影响。同时，可以对分支进行合并，合并后一个分支的修
改便可在另一分支上生效。总之，git是当今最优秀的版本控制工具！



### 安装：

https://git-scm.com/download/mac

Windows：根据官网提示，下载完无脑下一步，

macOS：如果是mac一般自带git，可以在终端输入：`git -v`查看是否显示版本号，有则说明已经安装

### 配置：

打开终端进行git初始化配置

​	全局配置name：`git config --global user.name "tanzhiqiang"` （--global全局）

​	全局配置email：`git config --global user.email "1973175974@qq.com"`

**name和email都是自己的，工作中为了区分代码是谁写的**

### 使用git：

<img src="/Users/mac/Desktop/vue-course/assets/image-20230202124617259.png" alt="image-20230202124617259" style="zoom:200%;" />

​	`git status` ：查看当前仓库的状态（提示：fatal: not a git repository (or any of the parent directories): .git==>说明不是git仓库）

​	`git init`：初始化仓库，会多出一个 .git 隐藏文件（这个文件用来存储代码版本的信息，有了.git就意味着项目现在已经被.git管理了，不希望.git管理时，只需删除.git文件即可）；

​	查看隐藏文件：

​			mac快捷键： Command + Shift + > 同时按住三个键即可显示隐藏文件，隐藏再操作一次；

**文件状态**
git中的文件有两种状态：未跟踪和已跟踪。未跟踪指文件没有被git所管理，已跟踪指文件已被
git管理。已跟踪的文件又有三种状态：未修改、修改和暂存。
**暂存**，表示文件修改已经保存，但是尚未提交到git仓库。
**未修改**，表示磁盘中的文件和git仓库中文件相同，没有修改。
**已修改**，表示磁盘中文件已被修改，和git仓库中文件不同。
可以通过`git status`来查看文件的状态

刚刚添加到项目中的文件处于未跟踪状态:

​	 **未跟踪--->暂存**：

​					`git add  <filename>` 将文件切换到暂存状态

​					`git add *`：把所有的未跟踪的文件切换的暂存状态

![image-20230202143238666](/Users/mac/Desktop/vue-course/assets/image-20230202143238666.png)

​	**暂存--->未修改**：

​					`git commit -m "xxxx"`  将暂存的文件存储到仓库中 

![image-20230202144318205](/Users/mac/Desktop/vue-course/assets/image-20230202144318205.png)

​	**未修改--->已修改**：

​					修改代码后，文件会变为修改状态

​					`git commit -a -m "xxxx"`: 提交所有已修改的文件（未跟踪的文件不会提交），相当于减少一步操作： `git add  *`

![image-20230202142612030](/Users/mac/Desktop/vue-course/assets/image-20230202142612030.png)

### 常用命令：

1. 重置文件

   ```bash
   git restore <filename>  #恢复文件
   git restore --staged <filename> #取消暂存文件
   ```

2. 删除文件

   ```bash
   git rm <filename>  #删除文件
   git rm <filename> -f #强制删除
   ```

​	3.移动文件或重命名一个文件

```bash
git mv <file> <newfile> #重命名文件
```

### **分支**

git在存储文件时，每一次代码代码的提交都会创建一个与之对应的节点，git就是通过一个一个的节点来记录代码的状态的。节点会构成一个树状结构，树状结构就意味着这个树会存在分支，默认情况下仓库只有一个分支，命名为master。在使用git时，可以创建多个分支，分支与分支之间相互独立，在一个分支上修改代码不会影响其他的分支。

```bash
git branch #查看当前分支
git branch <branch name> #创建新的分支
git branch -d <branch name> #删除分支
git branch <branch name> #切换分支
git switch -c <branch name> #创建并切换分支
```

在是实际开发中，都是在自己的分支上编写代码，代码编写完成后，再将自己的分支合并到主分支中

```bash
git merge <branch name> #合并分支  注意：一定要在主分支上操作合并分支
```

**变基(rebase)**

```bash
git rebase <branch name> #变基
```

在开发中除了通过merge来合并分支外，还可以通过变甚来完成分支的合并。
我们通过merge合并分支时，在提交记录中会将所有的分支创建和分支合并的过程全部都显示出来，这样当项目比较复杂，开发过程比较波折时，我必须要反复的创建、合并、删除分支。这样一来将会使得我们代码的提交记录变得极为混乱。
原理（变基时发生了什么）：
1.当我们发起变基时，git会首先找到两条分支的最近的共同祖先
2.对比当前分支相对于祖先的历史提交，并且将它们提取出来存储到一个临时文件中
3.将当前部分指向目标的基底
4.以当前基底开始，重新执行历史操作
变基和merge对于合并分支来说最终的结果是一样的！但是变基会使得代码的提交记录更整洁更清晰！注意！大部分情况下合并和变基是可以互换的，但是如果分支已经提交给远程仓库，那么这时进来能不要变基。

**远程仓库（remote)**
目前我对于git所有操作都是在本地进行的。在开发中显然不能这样的，这时我们就需要一个远程的git仓库。远程的git仓库和本地的本质没有什么区别，不同点在于远程的仓库可以被多人同时访问使用，方便我们协同开发。在实际工作中，git的服务器通常由公司搭建内部使用或是购买一些公共的私有git服务器。我们学习阶段，直接使用一些开放的公共git仓库。目前我们常用的库有两个：GitHub和Gitee(码云）
将本地库上传gitHub：

```bash
git remote add origin https://github.com/Code2022523/gie-demo.git
# git remote add <remote name> <url>
# origin 远程仓库分支的名字
git branch -M main
# 修改本地仓库分支的名字为main
git push -u origin main
# git push 将代码上传服务器上
```

将本地库上传gitee：

```bash
git remote add gitee https://gitee.com/tanzhiqiangS/git-demo.git
git push -u gitee main
```

`cat ~/.gitconfig`: 查看当前git绑定的账号和email等信息

**注意：如果gitee推送代码到远程服务器的时候需要查输入Username和Password**

Username：是邮箱 （1973175974@qq.com）不是用户名

Password：是登录密码（tzq257741）

**从远程仓库克隆代码**：git clone https://gitee.com/tanzhiqiangS/git-demo.git

**注意**：需要在一个文件夹下打开终端，克隆的代码会保存到当前文件夹下