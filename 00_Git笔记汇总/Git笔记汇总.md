# 汇总笔记



## 起步

### 版本控制简介

### 安装Git

[Git官方下载地址](https://git-scm.com/download)

如果下载慢可以使用这一个地址下载windows版本的[Git for win](https://npm.taobao.org/mirrors/git-for-windows/)

### 初次运行Git需要进行的配置

#### Git的`config`文件的介绍

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

1. `/etc/gitconfig` 文件: 包含==系统上每一个用户及他们仓库的通用配置==。 如果在执行 git config 时带上`--system` 选项，那么它就会读写该文件中的配置变量(由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它)。
2. `~/.gitconfig` 或` ~/.config/git/config` 文件：==只针对当前用户==。你可以传递 `--global`选项让 Git读写此文件, 这会==对你系统上所有的仓库生效==。
3. 当前使用仓库的 Git 目录中的 config 文件(即 `.git/config`): ==针对该仓库==。 你可以传递 --`local`选项让 Git 强制读写此文件，虽然默认情况下用的就是它(当然, 你需要进入某个 Git 仓库中才能让该选项生效)。

如果同时进行了上面这三种级别的配置，则会按照就近原则来生效，也就是3的配置会覆盖2的配置，2的配置会覆盖1的配置。

#### 使用`config`命令来进行Git的用户名和邮箱地址的配置

根据上面的介绍, 我们应该在初次启动Git时就对`/etc/gitconfig`文件或者是`~/.gitconfig`(`~/.config/git/config`)文件进行配置。其中一件必须要做的事情是设置你的用户名以及邮箱地址。这两个信息是必要且十分重要的，因为每一次Git的提交都要用到这些信息，它们会写到我们的每一次提交中不可更改。

也就是说，这个我们配置的用户名以及邮箱地址唯一地标识了我们的身份。配置方式如下：

1. 打开右键打开Git的命令行工具

   ![image-20200523181209947](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200523181209947.png)

2. 在其中键入如下代码

   ```bash
   git config --global user.name "your user name"
   git config --global user.email "your email address"
   ```

   - 这里说明以下，这个用户名和邮箱地址只是用于在本地库的提交起作用，用户名和邮箱地址和后面的远程库的用户名和密码没有必要联系，因此邮箱地址甚至可以是一个不存在的地址。
   - 同时以上的命令只需要执行一次，这个信息就可以在你的系统用户中在多个仓库中使用

3. 对于1.3.2中的第三个`config`的配置，需要我们先建立仓库才能进行，因为它只对某一个仓库生效，不是全局的，配置命令如下：

   ```bash
   git config [--local] user.name "user name"
   git config [--local] user.email "your email"
   ```

   其中方括号[]的内容为可选。也就是默认的不带参数的`config`命令就是只对当前仓库进行配置。

4. 我的配置信息

   ```
   git config --global user.name "Square John"
   git config --global user.email "1042009398@qq.com"
   ```

#### 检查配置信息

1. 可以使用`git config –list`命令来列出所有Git当时能找到的配置信息，如下所示

   ```bash
   helloworld@surface MINGW64 ~/Desktop
   $ git config --list
   diff.astextplain.textconv=astextplain
   filter.lfs.clean=git-lfs clean -- %f
   filter.lfs.smudge=git-lfs smudge -- %f
   filter.lfs.process=git-lfs filter-process
   filter.lfs.required=true
   http.sslbackend=openssl
   http.sslcainfo=C:/Git/mingw64/ssl/certs/ca-bundle.crt
   core.autocrlf=true
   core.fscache=true
   core.symlinks=false
   pull.rebase=false
   credential.helper=manager
   core.editor="C:\Users\helloworld\AppData\Local\Programs\Microsoft VS Code\Code.exe" --wait
   user.name=Square John
   user.email=1042009398@qq.com
   ```

   通过上面的命令可能会检查到重复的变量名，因为`.gitconfig`文件有多个。在这种有多个相同的变量的情况下，最后一个同名变量生效。

2. 我们还可以通过`git config <key>`来检查某一个变量的值。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop
   $ git config user.name
   Square John
   ```

3. 由于Git会从多个`.config`文件中读取同一个配置变量的不同值，这时候我们可以通过下面的命令查询该变量的==原始值==，并且会告诉我们哪一个文件最后设置了该值。例如

   ```bash
   $ git config --show-origin user.name
   file:C:/Users/helloworld/.gitconfig     Square John
   ```

### 获取帮助

1. 我们在使用Git时如果需要帮助，可以通过以下的三种方式找到Git命令的综合手册

   ```bash
   $ git help <verb>
   $ git <verb> --help
   $ man git-<verb>
   ```

   例如我们输入命令`git help config`，浏览器就会打开下面这个`config`帮助页面

   ```text
   file:///C:/Git/mingw64/share/doc/git-doc/git-config.html
   ```

   从这个链接来看，这个帮助文档是处于本地的。

2. 当然，如果我们并不需要这么详细的帮助信息，我们可以将`help`简化为`-h`，这时候就会在终端显示该命令的简略用法，如下所示

   ```bash
   $ git -h config
   unknown option: -h
   usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
              [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
              [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
              [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
              <command> [<args>]
   
   ```

   

### END



## Git基础

###  获取Git仓库的两种方式

1. 将尚未进行版本控制的本地目录转换为Git仓库
2. 从其他服务器==克隆==一个已存在的Git仓库

这上述的两种方式都可以让我们在本地获得一个工作就绪的Git仓库。

#### 将一个尚未进行版本控制的本地目录转换为一个Git仓库

1. 将命令行窗口切换路径到需要转化为Git仓库的目录，例如我想将`gitLocalRepository`目录作为一个Git仓库，那么我首先要切换到其中

   ```bash
   helloworld@surface MINGW64 ~/Desktop
   $ cd gitLocalRepository/
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository
   $
   ```

2. 然后执行如下代码，便可以将该目录初始化为一个本地Git仓库

   ```bash
   $ git init
   ```

   在命令执行完之后的输出如下

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository
   $ git init
   Initialized empty Git repository in C:/Users/helloworld/Desktop/gitLocalRepository/.git/
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

3. 然后我们可以打开刚才的目录查看到如下内容，结果如下

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ ll -la
   total 8
   drwxr-xr-x 1 helloworld 197609 0  5月 23 21:14 ./
   drwxr-xr-x 1 helloworld 197609 0  5月 23 21:10 ../
   drwxr-xr-x 1 helloworld 197609 0  5月 23 21:14 .git/
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   我们可以看到，在该目录下多了一个`.git`文件夹，这个文件夹就是用来存放Git的仓库配置信息的，我们没事最好别乱修改其中的内容。我们可以打开该目录看到以下内容

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository/.git (GIT_DIR!)
   $ ll -la
   total 11
   drwxr-xr-x 1 helloworld 197609   0  5月 23 21:14 ./
   drwxr-xr-x 1 helloworld 197609   0  5月 23 21:14 ../
   -rw-r--r-- 1 helloworld 197609 130  5月 23 21:14 config
   -rw-r--r-- 1 helloworld 197609  73  5月 23 21:14 description
   -rw-r--r-- 1 helloworld 197609  23  5月 23 21:14 HEAD
   drwxr-xr-x 1 helloworld 197609   0  5月 23 21:14 hooks/
   drwxr-xr-x 1 helloworld 197609   0  5月 23 21:14 info/
   drwxr-xr-x 1 helloworld 197609   0  5月 23 21:14 objects/
   drwxr-xr-x 1 helloworld 197609   0  5月 23 21:14 refs/
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository/.git (GIT_DIR!)
   ```

4. 通过上面的初始化操作，我们就将一个目录初始化为了一个Git本地仓库，如果该目录原本就有文件，我们应该立即对这些文件进行追踪并进行初始提交

   - 使用`git add <file>`对文件进行追踪
   - 使用`git commit <file>`对文件进行提交

#### 克隆现有仓库

1. 克隆现有仓库的指令为

   ```bash
   $ git clone <url>
   ```

   其中`<url>`是远程仓库的地址。通过该命令，相当于基本就把远程库的所有内容复制到本地，而不仅仅是最新版本的文件而已。也就是说，通过克隆的指令，你获得了远程库几乎所有内容。

   该指令会在命令行窗口船舰一个和远程Git仓库同名的目录作为本地库。

2. 例如，我们执行如下的指令

   ```bash
   helloworld@surface MINGW64 ~/Desktop
   $ git clone https://github.com/libgit2/libgit2
   ```

   就会在`Desktop`目录下建立了一个`libgit2`目录，该目录就是处于就绪状态的一个本地Git仓库，我们就可以在其中进行版本控制了

3. 当然，如果你不喜欢远程库的名字，我们还可以通过以下方式自定义一个名字

   ```bash
   $ git clone <url> <repository name you want>
   ```

4. Git支持多种数据传输协议

   - `http://`协议
   - `https://`协议
   - `git://`协议
   - `SSH`协议

### 记录每次更新到仓库

#### Git仓库中文件的状态

在我们的工作区目录中的文件不外乎两种状态，即==已跟踪==和==未跟踪==状态。

1. ==已跟踪==是指文件已经被纳入了版本控制的状态，在上一次快照中已经有它们的记录。在工作一段时间后，它们的状态可能是==未修改==，==已修改==或是==已放入暂存区==。简而言之，==已跟踪==的文件就是Git已知的文件。
2. 工作区中除了==已跟踪==的文件外的所有其他文件都是==未跟踪==文件。它们及不存在于上一次快照的记录中，也没有被放入暂存区。
3. 像通过初始化目录形成Git本地仓库的情况，对于目录中的文件，如果没有执行过将文件放入暂存区的指令，那这样的文件就是==未跟踪==的文件。
4. 而对于像通过克隆得到的仓库，在你还未进行任何修改前，其中的所有文件都属于==已跟踪==的文件。
5. 文件已经进行过提交，也就是在本地库中已经有它们的记录，而之后如果我们又编辑修改了它们，那这样的文件就会被Git标记为==已修改==文件。在工作过程中，我们可以选择性地将这样的文件放入==暂存区==，随后再提交所有暂存的修改到本地库。

#### 检查当前文件状态

1. 我们可以通过`git status`指令查看当前工作目录下的文件处于何种状态。

2. 在没有任何东西的时候，使用`git status`指令查看当前工作目录中的文件状态信息如下

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   
   No commits yet
   
   nothing to commit (create/copy files and use "git add" to track)
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   - 其中第一行的`master`指明了我们当前所处的分支，至于分支是什么我们后面再说。`On branch master`这一行也指出了我们当前所处的分支。
   - `No commits yet`表示我们还未进行过任何提交
   - `nothing to commit`说明我们当前的工作目录中没有任何可以提交的东西。
   - `(create/copy files and use "git add" to track)`这个提示我们我们可以通过创建或者是复制文件进来然后通过`git add`命令对其进行追踪
   - 通过上面这一示例告诉我们，我们可以通过读取`git status`指令执行后输出的信息来判断文件的状态

3. 现在我们在工作区目录中新建一个`text.txt`文件，在其中写入一行内容，具体如下

   ```text
   this is the first time to commit
   ```

4. 然后我们可以通过`git status`命令再次查看文件的状态变化，这时候，就变成下面这样

   ```bash
   $ git status
   On branch master
   
   No commits yet
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           test.txt
   
   nothing added to commit but untracked files present (use "git add" to track)
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   - 这时候我们可以看到，这和上面的内容的区别主要有下面的几个

   - `Untracked files:`告诉我们有未追踪的文件，具体为`test.txt`。然后还有一行提示是

     ```bash
     (use "git add <file>..." to include in what will be committed)
     ```

     这个就是提示我们，我们可以通过`git add <file>`指令将未追踪的文件加入到将要被提交的暂存区中

   - 然后下面这一行

     ```bash
     nothing added to commit but untracked files present (use "git add" to track)
     ```

     告诉我们，还没有任何一个已经添加到暂存区以能够用于提交的文件，但是有==未追踪==的文件的出现。括号中的内容还是提示我们可以如何将文件添加到暂存区

#### 跟踪新文件

1. 可以通过`git add <file>`指令对新文件进行跟踪。例如我们需要对新文件`test.txt`文件进行跟踪，我们就应该执行如下指令

   ```bash
   $ git add test.txt
   ```

2. 执行了上述指令之后再通过`git status`指令检查文件状态就会变成下面这样

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   
   No commits yet
   
   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)
           new file:   test.txt
   
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   - `Changes to be committed:`告诉我们有可以被转变为已提交状态的文件`test.txt`，该文件为新文件，也就是刚刚新建的。

   - 此时应注意，`(use "git rm --cached <file>..." to unstage)`这一句并不是说使用这个指令进行提交，而是说我们可以通过`git rm –cached <file> …`指令将文件从暂存区中撤回，如果此时我们执行该指令，然后再执行`git status`指令，那么状态就会回到上一个状态。具体如下

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git rm --cached test.txt
     rm 'test.txt'
     
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git status
     On branch master
     
     No commits yet
     
     Untracked files:
       (use "git add <file>..." to include in what will be committed)
             test.txt
     
     nothing added to commit but untracked files present (use "git add" to track)
     
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     ```

3. 实际上`git add`的参数是一个文件或者是一个目录，准确地说`git add`指令是这样的

   ```bash
   git add <file/dir>
   ```

   如果参数是一个目录，那么就会递归地将目录下的所有文件进行追踪

4. 我们还应该注意到，在我们进行添加到暂存区的操作（也就是`git add`）时，可能会看到这个警告的内容

   ```text
   warning: LF will be replaced by CRLF in test.txt.
   The file will have its original line endings in your working directory
   ```

   这个其实不是说我们指令的执行过程中出现了什么问题，一般我们不用管这个警告。

5. 也就是说，在执行了`git add <file/dir>`指令后，相应的文件就会被添加到暂存区中，然后这些文件也变成了==已追踪==状态。`Changes to be committed`表示这是已暂存状态。

#### 暂存已修改的文件

1. 现在我们假设我们已经有了一个状态为==已追踪==的文件，假如是`tracked.txt`。

2. 为了说明这种情况，我们需要先新建一个`tracked.txt`文件，然后使用`git add`指令将其加入到暂存区，然后再通过`git commit`指令将其加入本地库中。至于`git commit`指令，后面很快就会说到，这里我们就先不纠结它。（在此之前我们先将`test,txt`文件从暂存区中撤回）。

3. 然后我们对`tracked.txt`文件进行修改，然后再运行`git status`指令将会获得如下的信息

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   test.txt
   
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   tracked.txt
   
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   我们可以看到两个状态，一个是`Changes to be committed:`，另一个是`Changes not staged for commit:`

   - `Changes to be committed:`是指已经标记为暂存状态的文件，这些文件已经加入到暂存区但是还没有提交到本地库，所以可以用于提交到本地库。我们注意到，其中的`test.txt`文件为`new file`状态，也就是刚刚新建的还没有提交过的文件
   - `Changes not staged for commit:`是指==已追踪==文件被修改了但是还没有加入到暂存区以备提交。已经提交过了的文件，做出了修改但是修改之后没有添加到暂存区。然后我们可以注意到`tracked.txt`是`modified`状态，也就是这是这个文件不是刚新建的，而是原本就有而现在做了修改的

4. 随后我们将其添加到暂存区中，即执行`git add`指令

5. 而如果我们再将其添加到暂存区之后，再对其进行修改，然后执行`git status`指令，我们会获得如下结果

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   test.txt
           modified:   tracked.txt
   
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   tracked.txt
   
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   我们可以看到，`tracked.txt`文件同时处于两个状态，也就是`Changes to be committed:`和`Changes not staged for commit:`，即同时存在于暂存区和工作区。这就说明了

   - Git暂存的是你最后一次`git add`的版本的文件

   - 用于提交的文件也是最后一次加入到暂存区中的文件，而不是你工作目录中最新版本的文件

   - 也就是`git add`将文件存到暂存区，`git commit`将暂存区的版本文件存到本地库

   - 如果我们对文件进行了修改但是没有`git add`它，它就只会停留在工作区中

   - 而`git status`指令是将会进行的是这样的两两比较

     - 本地库的文件和暂存区的文件进行比较。如果二者不同，则该文件将会出现在`Changes to be committed`中
     - 暂存区的文件和工作区中的文件进行比较。如果二者不同，则该文件将会出现再`Changes not staged for commit`中

     上面的`stracked.txt`文件由于进行了两次修改，第一次修改然后加入暂存区，导致了本地库和暂存区中该文件的状态的不同，而第二次仅仅是进行了修改但是没有加入到暂存区，所以导致了暂存区和工作区中的文件版本的不同。所以最终结果就是该文件出现在了两个检测状态中。

6. 这时候就应该对`tracked.txt`再次进行`git add`保证暂存区中的文件是最新版本。我们执行了`git add`之后再执行`git status`就会得到以下结果

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   test.txt
           modified:   tracked.txt
   
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   ```

   这时候暂存区就有两个新版本的文件了，随后如果执行`git commit`指令，就会将它们一并提交到本地库中。

7. 这里我们需要说一下的就是这个`git add`指令。

   - 该指令是一个多功能的指令
   - 它可以将一个新文件添加到暂存区中进行追踪
   - 它可以将一个已追踪的文件放入暂存区
   - 还可以用于合并有冲突的文件将其标记为已解决状态
   - 也就是说我们应该把这一个指令理解为“精确地将内容添加到下一次提交中”，而不仅仅是“将文件添加到项目中”

#### 状态简览

1. 使用`git status`指令很详细但是未免有些繁琐

2. 此时我们可以使用`-s`或者是`--short`选项缩短状态命令的输出。例如

   - 如果我们使用的是`git status`指令，输出如下

     ```bash
     $ git status
     On branch master
     Changes to be committed:
       (use "git restore --staged <file>..." to unstage)
             modified:   tracked.txt
     
     Changes not staged for commit:
       (use "git add <file>..." to update what will be committed)
       (use "git restore <file>..." to discard changes in working directory)
             modified:   tracked.txt
     
     Untracked files:
       (use "git add <file>..." to include in what will be committed)
             test.txt
     ```

     

   - 而如果加入了`-s`或者是`--short`选项，输出就像下面这样

     ```bash
     $ git status -s
     MM tracked.txt
     ?? test.txt
     ```

     - 新添加的==未追踪==的文件前面是两个==??==

     - ==A==表示新的文件，==M==表示并非是新的而是经过修改的文件

     - 实际上，输出的左边应该分为两栏，如上的==MM==实际上是两栏

       - 左栏指明了暂存区的状态
       - 右栏指明了工作区的状态

     - 例如上面的==MM==

       - 第一个==M==说明该文件不是新文件，但是经过了修改且并添加到了暂存区中，但是还没有提交到本地库
       - 第二个==M==说明这不是一个新文件，但是经过了修改但是这个修改的版本仅仅是再工作区中进行了修改而没有添加到暂存区中
       - 简言之。第一个==M==就是说该文件不是新文件，但是本地库和暂存区中该文件版本内容）不同
       - 第二个==M==说明该文件不是新文件，但是暂存区和工作区中该文件版本（内容）不同

       


#### 忽略文件

1. 一般我们有些文件并不需要纳入Git的管理中，也不希望它们总出现在==未跟踪==文件列表。在这种情况下，我们可以通过建立一个`.gitignore`文件，然后在其中列出要忽略的文件模式。

2. `.gitignore`文件的格式规范如下

   - 所有空行都会被Git忽略
   - ==#==开头的行作为注释行被Git忽略
   - 可以使用标准的==glob==模式匹配，它会递归地应用在整个工作区
     - 所谓的==glob==模式就是==shell==所使用的简化了的正则表达式
     - `*`表示匹配0个或者多个任意字符
     - `[abc]`表示匹配如何一个列在方括号中的内容
     - `?`表示只匹配一个任意字符
     - `[0-9]`在方括号中使用一个`-`分隔连个字符，表示所有在这两个字符之间的所有字符都可以匹配
     - `**`表示匹配任意中间目录
   - 模式匹配可以以(==/==)开头防止递归
   - 模式匹配可以以(==/==)结尾指定目录
   - 要忽略指定模式以外的文件或者是目录可以在模式前加上(==!==)取反

3. 例子

   ```text
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

   

4. 实际上，我们大多数时候，直接可以到[github 的ignore网站](https://github.com/github/gitignore)去下载相应的模板就够了，而并不需要自己进行复杂的配置

5. `.gitignore`文件有可能就在一个仓库的根目录下面有且仅有一个文件。实际上在仓库下的子目录中也是可以创建相应的`.gitignore`文件来进行忽略文件控制的。

   - `gitignore`文件中的规则只作用在它所在的目录中，也就是只管该目录下的所有文件以及其下的所有子目录中的文件。
   - ==对于同一个规则有多个声明，应该遵循就近原则吧，这里没有实验，有空再补上。==



#### 查看以已暂存和未暂存的修改

1. 我们可以使用`git status`指令查看文件的状态，不过这个指令只告诉了我们哪些文件发生了改变，而如果想要知道具体修改了什么地方，这个指令是不够详细的

2. 如果我们想要知道具体修改了什么地方，则需要用到`git diff`指令。`git diff`通过文件补丁的形式能够更加详细地告诉我们具体是哪些地方发生了什么变化

   - `git diff` 想要查看尚未暂存的文件更新了什么内容，直接使用`git diff`指令就可以了。该指令通过对比的是工作区中的文件和暂存区中相应的文件的差异，然后将其输出。对比的是工作区中的最新更改但未加入到暂存区的文件和最后一次`git add`的暂存区中的文件差异。例如

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git diff
     warning: LF will be replaced by CRLF in tracked.txt.
     The file will have its original line endings in your working directory
     diff --git a/tracked.txt b/tracked.txt
     index 0851d86..f32a8d2 100644
     --- a/tracked.txt
     +++ b/tracked.txt
     @@ -1,2 +1,3 @@
      add the first line
      second time to modify it
     +third time to modify it
     ```

     通过该指令，我们可以看到工作区和暂存区中的`tracked.txt`文件的差异，工作区中的文件添加了一行==third time to modify it==，使用加号==+==显示

   - `git diff –cached` or `git diff –staged` 该指令对比的是最后一次提交到本地库的文件和暂存区的最后一次加入的相应文件之间的差异。例如

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git diff --cached
     diff --git a/tracked.txt b/tracked.txt
     index 8b13789..0851d86 100644
     --- a/tracked.txt
     +++ b/tracked.txt
     @@ -1 +1,2 @@
     -
     +add the first line
     +second time to modify it
     ```

     我们看到，暂存区中的`tracked.txt`和本地库中的`tracked.txt`文件相比，删除了一个空行（使用==-==表示删除行操作），增加了两行（用==+==表示增加行操作），分别是==add the first line==和==second time to modify it==

   - 综上，`git diff`比较的是工作区和暂存区中的各自最新的相应文件之间的差异，`git diff –cached` or `git diff –staged`比较的是暂存区和本地库的文件之间的差异。然后把差异部分输出显示。

#### 提交更新

1. 暂存区已就绪，现在我们就可以进行提交了，不过为了保险起见，我们最好再每一次提交之前先执行一下`git status`命令查看是否所有修改都已经加入暂存区，如果不是，可以先执行`git add`命令将未加入暂存区的更改加入其中

2. 使用`git commit`指令一次性提交暂存区中的所有文件。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   test.txt
           modified:   tracked.txt
   
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git commit
   hint: Waiting for your editor to close the file...
   [main 2020-05-24T09:41:33.343Z] update#setState idle
   [main 2020-05-24T09:42:03.355Z] update#setState checking for updates
   [main 2020-05-24T09:42:40.454Z] Error: net::ERR_TIMED_OUT
       at SimpleURLLoaderWrapper.<anonymous> (electron/js2c/browser_init.js:2623:21)
       at SimpleURLLoaderWrapper.emit (events.js:203:13)
   [main 2020-05-24T09:42:40.458Z] update#setState idle
   [master 9cf1dd4] this is the test.txt file first time to commit and the tracked.txt file is the first time to modify
    2 files changed, 4 insertions(+), 1 deletion(-)
    create mode 100644 test.txt
   ```

   - 执行了`git commit`命令后，会弹出我们设定的编辑器用于为本次提交写注释信息，其中默认的文本包含了再执行`git commit`之前执行`git status`的输出信息，但是是带注释符号的，也就是这些信息是给我们参考的，可以看到本次提交修改了哪些文件，但是这些文件并不会真正提交上去，只有哪些前面不带==#==的行才会被Git作为提交注释信息。具体如下

     - 弹出的原始文本信息

       ```text
       # Please enter the commit message for your changes. Lines starting
       # with '#' will be ignored, and an empty message aborts the commit.
       #
       # On branch master
       # Changes to be committed:
       #	new file:   test.txt
       #	modified:   tracked.txt
       #
       ```

       

     - 添加的注释信息

       ```text
       # Please enter the commit message for your changes. Lines starting
       # with '#' will be ignored, and an empty message aborts the commit.
       #
       # On branch master
       # Changes to be committed:
       #	new file:   test.txt
       #	modified:   tracked.txt
       #
       this is the test.txt file first time to commit
       and the tracked.txt file is the first time to modify
       ```

       

   - 如果我们想要更加详细的信息出现在编辑器中，我们可以添加`-v`选项，这时候在编辑器中出现的默认文本就是`git diff`指令的输出文本，例如执行如下指令

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git diff --cached
     diff --git a/test.txt b/test.txt
     index a75d7d4..20071db 100644
     --- a/test.txt
     +++ b/test.txt
     @@ -1,2 +1,3 @@
      this is the first time to commit
      this is the second time to edit this file
     +this is the third time to edit this file
     
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git commit -v
     hint: Waiting for your editor to close the file...
     [main 2020-05-24T09:59:19.027Z] update#setState idle
     [main 2020-05-24T09:59:49.039Z] update#setState checking for updates
     Aborting commit due to empty commit message.
     
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $
     ```

     其中弹出的编辑器的默认文本为

     ```text
     # Please enter the commit message for your changes. Lines starting
     # with '#' will be ignored, and an empty message aborts the commit.
     #
     # On branch master
     # Changes to be committed:
     #	modified:   test.txt
     #
     # ------------------------ >8 ------------------------
     # Do not modify or remove the line above.
     # Everything below it will be ignored.
     diff --git a/test.txt b/test.txt
     index a75d7d4..20071db 100644
     --- a/test.txt
     +++ b/test.txt
     @@ -1,2 +1,3 @@
      this is the first time to commit
      this is the second time to edit this file
     +this is the third time to edit this file
     ```

     也就是说带有`git diff -cached`的输出信息

3. `git commit -m “<message>”`将提交消息和命令放在同一行，通过`-m`选项，可以直接输入本次提交的注释信息，从而不用打开编辑器编辑。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git commit -m "for testing the command 'git commit -m' "
   [master 3c5e45b] for testing the command 'git commit -m'
    2 files changed, 2 insertions(+)
   ```

   通过`-m`选项可以直接在提交的时候写注释信息，而不会弹出编辑器界面。

#### 跳过使用暂存区域

每次先`git add`然后再`git commit`的做法虽然是十分周到的，但是有时候我们未免觉得有些繁琐。Git也考虑到了这个问题，所以提供了直接提交的方法。

Git提供了一个跳过使用暂存区的方式，直接将文件提交，只要在`git commit`命令中加上`-a`选项，Git就可以自动地把所有==已经跟踪过的==文件暂存起来一并提交，从而可以跳过`git add`这样一个环节。从上面的描述中我们似乎可以推出这一个选项只对==已追踪==的文件有效，而对新建的尚==未追踪==的文件不能使用，实际测试也是如此

```bash
helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
$ git commit -a -m "test 'git commit -a'"
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test_03.txt

nothing added to commit but untracked files present (use "git add" to track)
```

上面的指令就是对新建的还没有`git add`过的文件直接使用`git commit -a`指令出现的问题。因此，使用`-a`选项，前提是工作区中的所有文件都已经至少`git add`过。如果文件==已追踪==就可以使用该选项，示例如下：

```bash
helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
$ git commit -a -m "second time test the command 'git commit -a'"
warning: LF will be replaced by CRLF in test_03.txt.
The file will have its original line endings in your working directory
[master 97d25c5] second time test the command 'git commit -a'
 1 file changed, 1 insertion(+)
```

当然，这个命令很方便，但是官方文档说了：有时候这个选项会把不需要的文件添加到提交中，所以使用还是要小心。

#### 移除文件

1. 要从Git中移除某一个文件，就必须从已追踪清单中移除（确切地说是从暂存区中移除），然后提交。

2. `git rm <file>` 使用该指令会将相应的文件从暂存区和工作区中移除，这样今后该文件都不会在出现在==未追踪==文件清单中了。

3. 但是如果只是简单地从工作目录中手动删除了一个文件，则运行`git status`时该文件名就会出现在==Changes not staged for commit==部分（也就是未暂存清单中），例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ ls .
   test.txt  test_03.txt  test_rm.txt  tracked.txt
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   nothing to commit, working tree clean
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ rm test_rm.txt
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add/rm <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           deleted:    test_rm.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   为了不再对该文件进行追踪，还需要执行`git rm <file>`这一指令，这才能保证该文件在下一次提交之后不再被纳入到版本管理中。

4. 如果需要删除的是在之前已经修改过但是未加入到暂存区，或者是修改过并刚添加到暂存区但是还没有提交的文件，则需要`-f`选项（即force得首字母）。这是一种安全特性，防止误删尚未添加到快照得数据，这样得数据不能够被Git恢复。例如执行下面得操作如果没有`-f`就会出错

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ vim test_03.txt
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git rm test_03.txt
   error: the following file has local modifications:
       test_03.txt
   (use --cached to keep the file, or -f to force removal)
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git add test_03.txt
   warning: LF will be replaced by CRLF in test_03.txt.
   The file will have its original line endings in your working directory
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git rm test_03.txt
   error: the following file has changes staged in the index:
       test_03.txt
   (use --cached to keep the file, or -f to force removal)
   ```

   - 上面得指令先是修改了`test_03.txt`文件，然后再刚修改完文件之后就执行删除文件的操作，这时候就会出现错误==error: the following file has local modifications:==
   - 然后把该修改加入到暂存区但是还没有提交，然后就执行删除操作，照样会删错失败并出现==error: the following file has changes staged in the index:==
   - 这样的一种机制就尽可能保证了我们不会进行误删操作。因为Git中文件的删除都是只能删除工作区，暂存区和本地库版本一致的文件，因为这样的删除操作是可逆的，删除的文件数据可以恢复。而对于版本不一致的文件，必须加入`-f`选项，保证了你是确实要执行这种不可恢复的操作，因为这些文件的最新数据并没有提交到本地库。

5. 另外一种情况是，我们想要把文件从Git仓库中删除（实际上是从暂存区删除），但是仍然希望文件能保留在工作目录中。也就是说我们不想让Git追踪该文件但是我们又想让它存在仓库的目录中，实际上就类似那种应该要被忽略的但是忘记忽略了现在突然想起来要忽略的文件。对于这样的情况，我们可以使用`git rm –cached <file>`指令，该指令实际上就是将文件从暂存区中删除，而保留工作区中的文件，想要在后期不再管理此文件还是需要将其手动加入到`.gitignore`文件中。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git rm --cached test_03.txt
   rm 'test_03.txt'
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ vim test_03.txt
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           deleted:    test_03.txt
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           test_03.txt
   
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git commit -a -m "test 'git rm --cached '"
   [master 872ec7b] test 'git rm --cached '
    1 file changed, 3 deletions(-)
    delete mode 100644 test_03.txt
   ```

   实际上执行这一指令就是将文件从暂存区进行删除了，但是文件还保留在工作区中，文件还是可以被Git检测到

6. `git rm`指令可以删除目录，也可以使用==glob==模式

7. 上面说的似乎有些复杂，实际上可以总结如下：

   - 对于工作区，暂存区和本地库三个区中版本一致的文件可以执行`git rm`指令
   - 而执行`git rm <file>`指令会将文件从暂存区和工作区删除，执行之后文件不再存在，然后再提交，本地库就会记录该文件已经被删除，今后该文件就不再存在了，肯定就不能再对其进行版本控制了
   - `git rm –cached`指令就只是将文件从暂存区删除，但是工作区还保留着该文件。这一指令是对于那种忘记忽略某些文件时可以采取的操作，因为该操作不会将工作区中的文件删除，我们在执行该指令之后将这些文件手动添加到`.gitignore`文件中，今后Git就不再对其进行追踪了。但是如果我们不将其加入到`.gitigore`文件中，在今后它们还是要被Git检测到，也就是它们还是属于要就行版本控制的文件。这一指令实际上并不会使得执行了该指令后的文件自动被Git忽略。
   - 对于那些在工作区和暂存区中的版本和本地库的版本不一致的文件，如果要执行删除操作，必须要加上`-f`选项，表示强制删除。因为对于这样的文件，如果删除，使用Git是无法找回它们的所有历史数据的，因为最新的修改没有保存在本地库，所以要使用`-f`以确保你是在知道删除该文件的后果的情况下进行的操作

#### 移动操作

1. Git并不会显示跟踪文件的移动操作。因为如果在Git中对文件进行了重命名，文件中的数据并不会发生改变。但是Git却能够推断出这是一次重命名操作。

2. Git提供了一条命令用于移动文件和对文件重命名，这条命令如下

   ```bash 
   $ git mv <from_file> <to_file>
   ```

3. 执行这一条命令，示例如下

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git mv test_03.txt test_03
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           renamed:    test_03.txt -> test_03
   ```

   我们可以看到Git确实能够知道这是一次重命名操作。至于这是怎么实现的，我们到后面再说。

4. 实际上`git rm <from_file> <to_file>`指一条指令，相当于执行以下三条指令的结果

   ```bash
   mv <from_file> <to_file>
   git rm <from_file>
   git add <to_file>
   ```

   如果我们通过这样分开的操作，Git也能判断出这是一次重命名操作。所以说在Git中我们可以通过两种方式对文件进行重命名。但是明显使用`git mv`方便得多。分开操作实例如下

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ mv test_03 test03
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git rm test_03
   rm 'test_03'
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git add test03
   warning: LF will be replaced by CRLF in test03.
   The file will have its original line endings in your working directory
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           renamed:    test_03 -> test03
   ```

   我们可以看到，Git同样可以知道我们进行了一次重命名的操作。

5. 有时候我们可能要使用其他工具批量重命名。这时候我们要记得在提交之前删除旧文件名，添加新文件名。

#### 查看提交历史

1. 在几个进行了若干次修改和提交之后，或者是我们刚克隆了一个仓库我们可能想回顾一下提交的历史，这时候我们就可以通过`git log`命令实现这一种需求

2. 这里我们先补一下`git diff`的知识

   - 示例

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git diff
     warning: LF will be replaced by CRLF in test03.
     The file will have its original line endings in your working directory
     diff --git a/test03 b/test03
     index e0a9216..3ec31f6 100644
     --- a/test03
     +++ b/test03
     @@ -2,3 +2,4 @@ test "git commit -a"
      second time test "git commit -a"
      test rm -f
      git rm --cached
     +add a line
     ```

   - `git diff`输出的各行的含义

     - 如下这两行是由于设置了unix和win之间换行符的转换导致产生的警告，可以忽略

       ```text
       warning: LF will be replaced by CRLF in test03.
       The file will have its original line endings in your working directory
       ```

       

     - `diff --git a/test03 b/test03`

       - 表示的是输出的结果是git 格式的==diff==

       - 然后要比较的是变动前的a版本的`test03`和变动后的b版本的`test03`之间的差异

     - `index e0a9216..3ec31f6 100644`

       - `index e0a9216..3ec31f6`表示变动前后两个文件的哈希值
       - `100644`表示文件的权限，`644`表示这是一个普通文件

     - 下面这两行

       ```bash
       --- a/test03
       +++ b/test03
       ```

       - `--- a/test03` 前面带有==---==表示变动前的版本
       - 那么后一个就是带有==+++==就表示变动后的版本

     - `@@ -2,3 +2,4 @@ test "git commit -a"`

       - `@@ -2,3 +2,4 @@`分为两个部分，前半部分==-2, 3==表示这是变动前的版本==2, 3==表示是对比从变动前的版本的第2行开始之后的3行的内容。后半部分==+2, 4==表示对比的是变动后的版本的第2行开始的后4行
       - `test "git commit -a"`表示变动前版本提交时的注释说明

     - 下面这几行是差异对比的内容

       ```bash
        second time test "git commit -a"
        test rm -f
        git rm --cached
       +add a line
       ```

       - 前面带有==+==号表示改动后的版本相对改动前的版本增加的内容
       - 前面带有==-==号表示改动后的版本相对于改动前的版本删除的内容

3. 一个`git log`的简单示例

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git log
   commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master)
   Author: Square John <1042009398@qq.com>
   Date:   Mon May 25 08:33:31 2020 +0800
   
       renamed test_03 to test03
   
   commit 6b78b5e394c7cb2e587160f3fd11048ca09a6f92
   Author: Square John <1042009398@qq.com>
   Date:   Mon May 25 08:29:47 2020 +0800
   
       renamed test_03.txt to test_03
   
   commit 0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8
   Author: Square John <1042009398@qq.com>
   Date:   Mon May 25 08:19:18 2020 +0800
   ```

   - 执行该命令之后，我们会获得我们所有提交的历史记录
   - 每一条记录的显示格式为
     - ==commit 717a7d2979a28eb269a6397141f4c90a688bdba8 [(HEAD -> master)]==
       - commit之后的一串字符是每一次提交文件的SHA-1校验和
       - ==(HEAD -> master)==中`HEAD`所在的位置表示当前文件的版本，`master`表示当前所在分支
     - ==Author: Square John <1042009398@qq.com>==这一行明显就是提交的作者信息，包括作者名字和邮箱
     - ==Date:   Mon May 25 08:33:31 2020 +0800==这一行表示提交日期
     - ==renamed test_03 to test03==这一行就是我们的提交说明

4. `git log`的几个常用选项介绍

   1. `-p` or `--patch`显示每一次提交所引入的差异（按补丁的格式输出）

   2. `-num` 其中==num==表示一个正整数，表示最多显示==num==条记录

   3. 上两个选项的示例

      ```bash
      helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
      $ git log -p -6
      commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master)
      Author: Square John <1042009398@qq.com>
      Date:   Mon May 25 08:33:31 2020 +0800
      
          renamed test_03 to test03
      
      diff --git a/test_03 b/test03
      similarity index 100%
      rename from test_03
      rename to test03
      
      commit 6b78b5e394c7cb2e587160f3fd11048ca09a6f92
      Author: Square John <1042009398@qq.com>
      Date:   Mon May 25 08:29:47 2020 +0800
      
          renamed test_03.txt to test_03
      
      diff --git a/test_03.txt b/test_03
      similarity index 100%
      rename from test_03.txt
      rename to test_03
      
      commit 0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8
      Author: Square John <1042009398@qq.com>
      Date:   Mon May 25 08:19:18 2020 +0800
      
          commit test_03.txt
      
      diff --git a/test_03.txt b/test_03.txt
      new file mode 100644
      index 0000000..e0a9216
      --- /dev/null
      +++ b/test_03.txt
      @@ -0,0 +1,4 @@
      +test "git commit -a"
      +second time test "git commit -a"
      +test rm -f
      +git rm --cached
      ```

      我们可以看到，加入了`-p`选项之后，输出中就多了`git diff`的输出部分

   4. `--stat`输出每次提交的粗略统计信息。例如

      ```bash
      $ git log -6 --stat
      commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master)
      Author: Square John <1042009398@qq.com>
      Date:   Mon May 25 08:33:31 2020 +0800
      
          renamed test_03 to test03
      
       test_03 => test03 | 0
       1 file changed, 0 insertions(+), 0 deletions(-)
      
      commit 6b78b5e394c7cb2e587160f3fd11048ca09a6f92
      Author: Square John <1042009398@qq.com>
      Date:   Mon May 25 08:29:47 2020 +0800
      
          renamed test_03.txt to test_03
      
       test_03.txt => test_03 | 0
       1 file changed, 0 insertions(+), 0 deletions(-)
      
      commit 0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8
      Author: Square John <1042009398@qq.com>
      Date:   Mon May 25 08:19:18 2020 +0800
      
          commit test_03.txt
      
       test_03.txt | 4 ++++
       1 file changed, 4 insertions(+)
      ```

      增加了`--stat`选项之后，就不再输出每次提交引入的差异的详细信息了，而是显示其中的粗略的统计信息。例如下面这几行

      ```bash
       test_03.txt | 4 ++++
       1 file changed, 4 insertions(+)
      ```

      表示本次提交在`test_03.txt`中增加了4行，本次提交有一个文件发生了改变，增加了4行

   5. `--pretty`可用于指定不同格式用于输出提交历史。它有下面几个子选项

      - `oneline`表示一条记录使用一行来进行输出。示例

        ```bash
        helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
        $ git log --pretty=oneline
        717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master) renamed test_03 to test03
        6b78b5e394c7cb2e587160f3fd11048ca09a6f92 renamed test_03.txt to test_03
        0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8 commit test_03.txt
        872ec7b2a068b676dc0a62d1c34b211be869bf3b test 'git rm --cached '
        6fb5ea099be4353c4a15a604127dfd30ad5566ea modified test_03.txt
        5467b52f33083d64b49931ec7fcae79a71ac7699 delete test_rm.txt
        dbf3a645b1afd2164aa670733e8304bae0cde601 test rm command
        97d25c59f4583ed4e532466113e47ee7244b4c3c second time test the command 'git commit -a'
        ef3fc04aa669d3813257bf652d5f782bab59f31c create the test_03.txt file
        3c5e45bfcc16c7172fdf4ef76a0500a4cc92f39b for testing the command 'git commit -m'
        41fe2d33654009b729c4d8988374282bad69db0f this is the second time to edit this file
        9cf1dd4c8558bcfff0e4cf88d923313d36e5671f this is the test.txt file first time to commit and the tracked.txt file is the first time to modify
        b958cb41c923adde860490de6660481e4dc30825 create the tracked.txt file
        ```

        输出格式为完整的==SHA-1==校验和以及提交说明

      - `short`表示使用简短的形式进行输出

      - `full`和`fuller`顾名思义，就是详细输出和更详细输出

      - `format`可以用于定制输出的格式

        - `format`的常用选项如下表

          | 选项 |                        说明                        |
          | :--: | :------------------------------------------------: |
          |  %H  |                  提交的完整哈希值                  |
          |  %h  |                  提交的简略哈希值                  |
          |  %T  |                   树的完整哈希值                   |
          |  %t  |                   树的简略哈希值                   |
          |  %P  |                 父提交的完整哈希值                 |
          |  %p  |                 父提交的简略哈希值                 |
          | %an  |                       作者名                       |
          | %ae  |                    作者电子邮箱                    |
          | %ad  | 作者的修订日期（可以使用 --date= 选项 来定制格式） |
          | %ar  |      作者的修订日期（按多久以前的方式来显示）      |
          | %cn  |                    提交者的名字                    |
          | %ce  |                  提交者的电子邮箱                  |
          | %cd  |               提交的日期（参照上面）               |
          | %cr  |                     提交的日期                     |
          |  %s  |                      提交说明                      |

          这里我们应该可以注意到，修改者和提交者可能不是同一个人

        - `format`的示例

          ```bash
          helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
          $ git log --pretty=format:"hash:%h ; authorname:%an ; email:%ae ; %s"
          hash:717a7d2 ; authorname:Square John ; email:1042009398@qq.com ; renamed test_03 to test03
          hash:6b78b5e ; authorname:Square John ; email:1042009398@qq.com ; renamed test_03.txt to test_03
          hash:0fe3c8d ; authorname:Square John ; email:1042009398@qq.com ; commit test_03.txt
          hash:872ec7b ; authorname:Square John ; email:1042009398@qq.com ; test 'git rm --cached '
          hash:6fb5ea0 ; authorname:Square John ; email:1042009398@qq.com ; modified test_03.txt
          hash:5467b52 ; authorname:Square John ; email:1042009398@qq.com ; delete test_rm
          ```

          

      - `graph`通过增加一些==ASCII==字符串来形象地展示分支和合并历史。需要和`--pretty`的`--oneline`或者是`--format`结合使用

5. `git log`常用选项

   |      选项       |                      说明                      |
   | :-------------: | :--------------------------------------------: |
   |       -p        |        按补丁格式显示每个提交引入的差异        |
   |     --stat      |          显示提交的文件修改的统计信息          |
   |   --shortstat   |      只显示--stat中最后的行数添加删除统计      |
   |   --name-only   |       仅在提交信息后显示已修改的文件清单       |
   |  --name-status  |         显示新增、修改和删除的文件清单         |
   | --abbrev-commit |         仅显示SHA-1校验和的前几个字符          |
   | --relative-date | 使用较短的相对时间而不是完整的日期格式显示日期 |
   |     --graph     |    在日志旁使用ASCII图形显示分支与合并历史     |
   |    --pretty     |            自定义日志的输出显示格式            |
   |    --oneline    |   --pretty=oneline --abbrev-commit合用的简写   |

   - `--name-status`示例

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git log --name-status -6
     commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master)
     Author: Square John <1042009398@qq.com>
     Date:   Mon May 25 08:33:31 2020 +0800
     
         renamed test_03 to test03
     
     R100    test_03 test03
     
     commit 6b78b5e394c7cb2e587160f3fd11048ca09a6f92
     Author: Square John <1042009398@qq.com>
     Date:   Mon May 25 08:29:47 2020 +0800
     
         renamed test_03.txt to test_03
     
     R100    test_03.txt     test_03
     
     commit 0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8
     Author: Square John <1042009398@qq.com>
     Date:   Mon May 25 08:19:18 2020 +0800
     
         commit test_03.txt
     
     A       test_03.txt
     
     commit 872ec7b2a068b676dc0a62d1c34b211be869bf3b
     Author: Square John <1042009398@qq.com>
     Date:   Sun May 24 20:59:18 2020 +0800
     
         test 'git rm --cached '
     
     D       test_03.txt
     
     commit 6fb5ea099be4353c4a15a604127dfd30ad5566ea
     Author: Square John <1042009398@qq.com>
     Date:   Sun May 24 20:56:53 2020 +0800
     
         modified test_03.txt
     
     M       test_03.txt
     
     commit 5467b52f33083d64b49931ec7fcae79a71ac7699
     Author: Square John <1042009398@qq.com>
     Date:   Sun May 24 20:35:38 2020 +0800
     
         delete test_rm.txt
     
     D       test_rm.txt
     ```

     - ==R100==表示重命名
     - ==A==表示新增加文件
     - ==D==表示删除文件
     - ==M==表示修改文件

   - `--relative-date`示例

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git log --relative-date -6
     commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master)
     Author: Square John <1042009398@qq.com>
     Date:   3 hours ago
     
         renamed test_03 to test03
     
     commit 6b78b5e394c7cb2e587160f3fd11048ca09a6f92
     Author: Square John <1042009398@qq.com>
     Date:   3 hours ago
     
         renamed test_03.txt to test_03
     
     commit 0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8
     Author: Square John <1042009398@qq.com>
     Date:   3 hours ago
     
         commit test_03.txt
     
     commit 872ec7b2a068b676dc0a62d1c34b211be869bf3b
     Author: Square John <1042009398@qq.com>
     Date:   14 hours ago
     
         test 'git rm --cached '
     
     commit 6fb5ea099be4353c4a15a604127dfd30ad5566ea
     Author: Square John <1042009398@qq.com>
     Date:   14 hours ago
     
         modified test_03.txt
     
     commit 5467b52f33083d64b49931ec7fcae79a71ac7699
     Author: Square John <1042009398@qq.com>
     Date:   15 hours ago
     
         delete test_rm.txt
     ```

     

   - `--oneline`示例

     ```bash
     helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
     $ git log --oneline
     717a7d2 (HEAD -> master) renamed test_03 to test03
     6b78b5e renamed test_03.txt to test_03
     0fe3c8d commit test_03.txt
     872ec7b test 'git rm --cached '
     6fb5ea0 modified test_03.txt
     5467b52 delete test_rm.txt
     dbf3a64 test rm command
     97d25c5 second time test the command 'git commit -a'
     ef3fc04 create the test_03.txt file
     3c5e45b for testing the command 'git commit -m'
     41fe2d3 this is the second time to edit this file
     9cf1dd4 this is the test.txt file first time to commit and the tracked.txt file is the first time to modify
     b958cb4 create the tracked.txt file
     ```

     

6. 日志的限制输出长度

   1. `-num` 只显示前==num==个提交历史。我们要注意，这里所说的前==num==个是最新的==num==个，而且是按照时间降序排列，也就是时间越大（越新）排在越前面，最新版排在第一个位置，以此类推。实际上`git log`的默认输出的排序方式就是这样

   2. `--since`表示自某一个日期开始其后的所有版本

      - 示例

        ```bash
        helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
        $ git log --since=10.hours --relative-date
        commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (HEAD -> master)
        Author: Square John <1042009398@qq.com>
        Date:   3 hours ago
        
            renamed test_03 to test03
        
        commit 6b78b5e394c7cb2e587160f3fd11048ca09a6f92
        Author: Square John <1042009398@qq.com>
        Date:   3 hours ago
        
            renamed test_03.txt to test_03
        
        commit 0fe3c8d1ccc682d907c2627cec27ac3f6f1d7fa8
        Author: Square John <1042009398@qq.com>
        Date:   3 hours ago
        
            commit test_03.txt
        ```

        `--since=10.hours`表示自10个小时前之后的所有提交记录，也就是前10个小时的提交历史。

      - 可用格式

        该选项可用格式十分丰富。可用`2020-05-20`这样的格式来表示某一天，也可以使用`3 minutes ago`这样的相对时间格式

   3. `--until` 和`--since`正好相反，`--until`表示要显示的是从最开始一直到某一个时间点为止的所有提交历史。示例

      ```bash
      helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
      $ git log --until=10.hours --relative-date
      commit 872ec7b2a068b676dc0a62d1c34b211be869bf3b
      Author: Square John <1042009398@qq.com>
      Date:   14 hours ago
      
          test 'git rm --cached '
      
      commit 6fb5ea099be4353c4a15a604127dfd30ad5566ea
      Author: Square John <1042009398@qq.com>
      Date:   14 hours ago
      
          modified test_03.txt
      
      commit 5467b52f33083d64b49931ec7fcae79a71ac7699
      Author: Square John <1042009398@qq.com>
      Date:   15 hours ago
      
          delete test_rm.txt
      
      commit dbf3a645b1afd2164aa670733e8304bae0cde601
      Author: Square John <1042009398@qq.com>
      Date:   15 hours ago
      
          test rm command
      
      commit 97d25c59f4583ed4e532466113e47ee7244b4c3c
      Author: Square John <1042009398@qq.com>
      Date:   15 hours ago
      
          second time test the command 'git commit -a'
      
      commit ef3fc04aa669d3813257bf652d5f782bab59f31c
      Author: Square John <1042009398@qq.com>
      Date:   15 hours ago
      
          create the test_03.txt file
      ```

      上面的例子就显示了凑从提一次提交知道10个小时之前的所有提交记录。

7. 过滤匹配指定条件的提交记录

   1. `--author`选项表示只显示指定作者的提交记录

   2. `--grep`选项用于指定搜索关键字，只显示==提交说明中==包含指定关键字的提交记录

   3. 我们可以同时指定多条`--author` and `--grep`过滤条件

      - 默认只要提交记录只要有一个条件匹配就会被显示出来
      - 添加`--all-match`选项之后只有那些满足所有过滤条件的提交记录才会被显示出来
      - 也就是对于具有多个过滤条件的情况，默认使用==||(或)==的逻辑关系，只要满足其中一个条件就会被输出。`--all-match`就是==&&(与)==的逻辑关系，只有满足所有条件才会被输出

   4. `-S` 它接受一个字符串参数，并且只会显示那些==文件数据中==增加或者删除了==包含该字符串==的提交历史记录。

   5. `--path` ==path==指定一个路径，只显示该路径下的目录和文件的提交历史记录。该选项应该放在最后面

   6. 用于限制日志输出长度的选项总结如下表

      |        选项         |                             说明                             |
      | :-----------------: | :----------------------------------------------------------: |
      |      --author       |                 只显示指定作者修改的提交记录                 |
      |       --grep        |     只显示提交说明中包含--grep选项指定的关键字的提交记录     |
      |        -num         |                  只显示最后的num个提交记录                   |
      | --since or --after  |                 只显示指定日期之后的提交记录                 |
      | --until or --before |                 只显示指定日期之前的提交记录                 |
      |     --committer     |                  只显示指定提交者提交的记录                  |
      |         -S          | 只显示文件数据中添加或者是删除了包含-S选项指定的字符串的提交记录 |

      

8. `--no-merges`隐藏合并提交的历史记录

   

### 撤销操作

在本节中我们将学习几个撤销我们所做的修改的工具。注意，其中有些撤销操作是不可逆的，这是在Git中会因为操作失误而导致前面所作的工作丢失的几个少有的地方之一。

#### 修补提交

`$ git commit --amend` 这个命令会将暂存区中的文件提交。如果自上次提交以来，你对文件还没有做任何修改，那么Git对文件的快照不变，使用该命令修改的只是提交信息。

1. 通过该指令提交，本次的提交说明会覆盖上一次的提交说明
2. 最终你只有一个提交，第二次的提交将代替第一次提交的结果
3. 使用该命令，其效果就好像前一次的提交从未出现过一样，==它不会出现在仓库的历史版本中==
4. 修补提交最大的价值是可以稍微改进你最后的提交。而不必因为像由于忘记暂存几个文件而需要对这些文件进行多次无修改的提交。
5. 试想一下，本来你是想一次性提交三个文件，结果你对文件修改完了，但是只将其中的一个文件加入了暂存区，然后提交了这个文件，提交之后你才意识到忘记将另外两个放入暂存区而导致内有提交。随后你又将另外一个文件放入暂存区而忘记了另外一个文件（真是夸张），然后又一次提交，然后又想起还有一个，然后再重复。像这样，提交了三次，但是在三次提交中，我们只在第一次提交前修改了文件，本来可以一次性提交作为一个版本，现在却成了三个版本。这样的状况发生得过多，就会导致仓库版本过多且过于繁琐。所以，`git commit --amend`指令的价值就在这里，可以覆盖掉前面一些比较没有意义的提交。

#### 取消暂存的文件

`git reset HEAD <file>` 指令用来从暂存区撤销对该文件的暂存。但是我们要知道这是一个危险的命令。这里就先学习这一个，后面会有关于`reset`指令的详细介绍。

#### 撤销对文件的修改

​	`git checkout -- <file>`命令可以用来撤销我们在工作区对文件所做的所有修改。这是一个危险的命令，一旦对某个文件执行了该指令，则这个文件在工作区中所做的任何修改都会消失，Git会使用最近提交的版本覆盖它。

​	除非你自己确实清楚不想要那个文件在工作区中所做的所有修改了，否则就不要使用它。

​	如果我们想保留对那个文件的修改但是现在仍然要撤销这些修改，我们可以使用后面将会学到的==Git分支==的做法，这通常是更好的做法。

​	我们要清楚，在Git种几乎所有的已提交的内容都是可以恢复的，甚至是上面所说的`git commit --amend`指令覆盖的提交记录。但是，任何我们还未提交的内容一旦消失，我们很有可能就再也找不回来了。

### 远程仓库的使用

#### 查看远程仓库

1. 通过`git remote`命令可以列出我们指定的所有的远程服务器的简写。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/temp (master)
   $ git remote
   cono
   ```

   

2. 添加`-v`选项可以列出所有的我们指定的远程仓库的简写和对应的URL。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/temp (master)
   $ git remote -v
   cono    https://github.com/srevinsaju/conozco.git (fetch)
   cono    https://github.com/srevinsaju/conozco.git (push)
   ```


#### 添加远程仓库

1. 使用`git clone`命令就可以自动将一个远程仓库克隆到当前目录下，具体的命令为

   ```bash
   $ git clone [<repository_name>] <remote_url>
   ```

   - 如果不指定仓库名称，就会使用远程仓库的名称。通过该指令，Git会默认为该远程仓库建立一个叫做==origin==的简写名称
   - 该命令是将远程库完整地拷贝到本地。克隆完成后我们就获得了一个处于工作状态的Git仓库，可以直接就在本地进行修改。

   

2. `git remote add <simple_name> <remote_url>` 该命令用于添加一个新的Git远程仓库，其中==simple_name==是该仓库的简写名称。**注意**：这里所说的以及上面所说的远程仓库的简写名称的意思是说今后我们可以通过该简写代替远程仓库的url和远程仓库进行交换数据。

   - 实际上，添加一个远程仓库的意思就是将远程仓库的URL记录到本地，同时为其起了一个简写名称。添加了一个远程仓库和克隆一个远程仓库是由很大区别的

     - ==添加一个远程仓库==的意思是说在本地库的基础上，通过`git remote add`命令，将一个远程仓库的URL记录下来，并用一个简写名称代表该URL。此后我们就可以访问该远程仓库了，如果我们有写入的权限，我们还可以修改该远程库。==但是添加了一个远程仓库并没有直接将远程库的内容下载到本地，只是在本地库增加了一个有关该远程库的记录，相当于我们现在知道了这样一个远程库的存在。要添加一个远程库，首先要在本地有一个仓库。==
     - ==克隆一个远程库==就是直接将远程库上的所有内容都复制到本地，直接在本地获得一个和远程库一样的本地库。同时该本地库会默认添加了一个简写为==origin==de 远程库，该远程库就是所克隆的远程库

   - `git remote add`仅仅是使得我们知道了有某一个远程库的存在，我们想要获得其中的内容，我们还有执行`git fetch <remote_repository>`这一个命令，其中==remote_repository==可以是一个远程库的链接，也可以是其简写名称。

   - 但是执行了`git fetch`命令只是将远程库的数据下载到了本地，它并不会自动将远程库的分支内容和本地库的分支内容合并起来。我们可以通过本地访问`<remote_repository>/branch_name`这样的方式访问到该远程库的某一个分支的内容

   - 如果需要将`git fetch`获得的远程库某一个分支的内容与本地库的某一分支内容进行合并，需要使用`git merge`命令。假设当前处于本地库的`master`分支（默认），然后我们想将远程库的`master`分支与其合并，则应该在执行了`git fetch`之后执行

     ```bash
     $ git merge <simple_name/master>
     ```

     ==simple_name==为远程库简写。

   - `git pull <simple_name>`指令就相当于`git fetch <simple_name>`加`git merge <simple_name/cur_branch>`。如果我们设置了当前分支跟踪远程分支该，指令在下载完远程库的内容到本地之后自动将远程库的被跟踪分支的内容和本地的当前分支的内容进行合并。这一指令显然更加简洁，但是如果合并时有冲突，还是需要进行手动合并。


#### 从远程仓库中抓取和拉取

1. `git fetch <remote_repository>`用于从远程仓库抓取内容到本地，但是不会合并内容，合并内容需要`git merge`
2. `git pull <remote_repository>`用于从远程仓库拉取内容，并且将拉取的远程库中的被当前本地库中分支跟踪的分支与本地库的当前分支的内容进行合并

#### 推送到远程仓库

1. `git push <remote_repository> <master>`命令用于将本地库中的==master==分支推送到==remote_repository==远程库中
2. 只有当你拥有==remote_repository==的写入权限，并且在你最后一次获取该远程库的数据之后还没有人进行过推送的情况下，你的推送才会被远程库服务器接受。只要其中一个条件不满足，就会被拒绝。对于第二种情况，就是在你推送之前已经有人进行过推送，那么我们想要推送我们的内容，必须先要拉取远程库的最新数据，然后在这个最新版本上再进行修改之后才能推送成功。

#### 查看某个远程仓库

1. `git remote show <remote_repository>`命令用来查看==remote_repository==远程库的信息。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/temp (master)
   $ git remote show cono
   * remote cono
     Fetch URL: https://github.com/srevinsaju/conozco.git
     Push  URL: https://github.com/srevinsaju/conozco.git
     HEAD branch: master
     Remote branch:
       master tracked
     Local ref configured for 'git push':
       master pushes to master (up to date)
   ```

   

2. 该命令会向我们展示如下信息

   - 我们抓取和推送的远程仓库的URL
   - 我们当前处于远程库中的==[master]==分支
   - 分支的跟踪信息，例如上面的例子中，==master==分支就正处于被跟踪状态
   - 该指令还会告诉我们哪些远程分支不在本地，哪些分支被远程服务器移除了
   - 我们通过==push==以及==pull==命令可以进行自动合并的相应的本地和远程分支组合

#### 远程仓库的重命名和移除

1. `git remote rename <from_name> <to_name>`命令用来对某一个远程库进行重命名。
   - 但是需要注意的是，这个命令是更改远程库在本地的简写名称。该命令不会真的将远程服务器上的远程库名字改了，而只是本地保存的对远程库的简写名称改了，并且会将该远程库在本地的所有引用到该简写名称的地方都改了。比如说当初我们使用`fetch`拉取了一个远程库的内容，在没有重命名之前我们通过`<from_name/master>`这样的方式访问==master==分支的内容，但是重命名之后要将==from_name==换成==to_name==
   - 综上，该命令实际上也就是改了远程库的简写名称，今后所有需要使用==from_name==的地方都将要被改为使用==to_name==
2. 如果我们想要移除一个远程库，可以使用`git remote remove/rm <remote_repository>`命令来移除一个远程库。同样需要注意的是：==该命令只是将本机上的与远程库相关的所有内容从本机上删除了，而不是将远程库从远程服务器上删除了==
3. 总结：上面的不管是对远程库进行重命名也好还是移除远程库也好，说的都是将本机上有关远程库的信息进行修改，这些操作都不会对远程服务器上的远程库产生任何影响。==重命名只是更改了本机对远程库的称呼，移除只是将远程库的内容从本机上移除==

### 打标签

我们可以在某些提交中打上一个标签，以示本次提交的重要性。比如说，我们在某些提交中打上这样的标签==v1.0 , v2.0==来表示发布节点。

#### 列出标签

1. `git tag`命令用于列出当前仓库中所有的标签。标签按照名字排序而不是按照时间排序

2. `git tag -l/--list <regex>`可以按照指定的模式列出符合条件的标签，例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git tag
   v0.9
   v1.0
   v1.0.0-lw
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git tag -l v1.0
   v1.0
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git tag --list v1.*
   v1.0
   v1.0.0-lw
   ```

   - 使用`git tag`命令会将完整的标签列表给列举出来。而增加了`-l/--list`选项之后就只列出符合其后的模式字符串==regex==的标签
   - 注意：==使用通配模式必须要有`-l/--list`选项==

#### 创建标签

Git支持两种标签：==轻量标签(lightweight)==和==附注标签(annnotated)==

##### 附注标签

1. 附注标签是存储在Git数据库中的一个完整对象。其中包括

   - 打标签者的名字
   - 打标签者的电子邮箱地址
   - 打标签的日期和时间
   - 一个标签信息

2. 附注标签是可以==被校验==的，并且可以使用GUN Privacy Guard(GPG)签名并验证。我们通常建议打附注标签。

3. 打附注标签的命令很简单，就是在`tag`指令中加入`-a`选项，具体如下

   ```bash
   $ git tag -a tagName -m "tag message"
   ```

   `-m`选项就是用于指定标签信息的，如果没有该选项，就会和提交的时候一样，会跳出编辑器要求你输入标签信息

4. 可以使用`git show <tagName>`来查看某一个标签的标签信息以及本次提交的提交信息。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git show v1.2.0
   tag v1.2.0
   Tagger: Square John <1042009398@qq.com>
   Date:   Tue May 26 10:15:36 2020 +0800
   
   this's version 1.2.0
   
   commit b5a7b0648a22c54c12eb20d9978c860196a1b43e (HEAD -> master, tag: v1.2.0)
   Author: Square John <1042009398@qq.com>
   Date:   Tue May 26 10:13:16 2020 +0800
   
       in test.txt add a line ,to verify tag command
   
   diff --git a/test.txt b/test.txt
   index 20071db..02aaa68 100644
   --- a/test.txt
   +++ b/test.txt
   @@ -1,3 +1,4 @@
    this is the first time to commit
    this is the second time to edit this file
    this is the third time to edit this file
   +git tag git tag git tag -a v1.2 -m "hhh"
   diff --git a/test03 b/test03
   index e0a9216..3ec31f6 100644
   --- a/test03
   +++ b/test03
   @@ -2,3 +2,4 @@ test "git commit -a"
    second time test "git commit -a"
    test rm -f
    git rm --cached
   +add a line
   ```

   - 第4行是打标签的人的名字和email地址
   - 第5行是打标签的时间
   - 第7行是标签信息
   - 其后是本次提交的信息以及`git --diff`的差异信息

##### 轻量标签

1. 轻量标签本质上就是将提交校验和存储到一个文件中，此外没有保存任何其他的东西。相当于一个临时的标记

2. 打一个轻量标签不需要任何额外的选项，只要在`git tag`之后跟上标签名就可以了，即

   ```bash
   $ git tag <tagName>
   ```

   

3. 使用这样的方式打标签，使用`git show <tagName>`命令我们不会看到任何有关改标签的标签信息。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git show v1.0.0-lw
   commit 717a7d2979a28eb269a6397141f4c90a688bdba8 (tag: v1.0.0-lw, tag: v1.0)
   Author: Square John <1042009398@qq.com>
   Date:   Mon May 25 08:33:31 2020 +0800
   
       renamed test_03 to test03
   
   diff --git a/test_03 b/test03
   similarity index 100%
   rename from test_03
   rename to test03
   ```

   该命令输出的只有本次提交的信息。

#### 后期打标签

Git支持对过往的提交记录打标签

1. 我们只需要使用`git log`命令列出相应的提交记录，然后获知提交记录的校验和（或者是部分校验和）
2. 然后就只需要在==对当前版本打标签的命令==后面加上需要打标签的==历史提交记录的校验和==就可以了
   - 打附注标签`git tag -a <tagName> -m “<tag msg>” <checksum>`。其中==checksum==为需要打标签的提交记录的校验和
   - 打轻量标签同理

#### 共享标签

1. 默认情况下，使用`git push`命令并不会将在本地打的标签传送到远程仓库的远端服务器去。
2. 必须要将标签显式推送到远端服务器上，使用`git push <remoteRepository> <tagName>`命令可以将指定的标签推送到远端服务器上
3. 但是如果一次一个这么推送，如果标签过多就会很麻烦。此时可以使用`git push <remoteRepository> --tags`一次性将所有本地建立的标签推送到远端服务器上。此后，被人克隆该远程库上的内容或者是从中拉取数据，都会得到这些标签。
4. `git push <remoteRepository> --tags`不会区分这些标签是轻量标签还是附注标签，两者都会被推送上去。没有任何一个选项支持分开将两种类型的标签分别批量上传。

#### 删除标签

1. 可以使用`git tag -d <tagName>`来从本地仓库上删除一个标签，但是这并不会将远端服务器上的远程库上的相应标签也删除。

2. 要从远程服务器上的相应仓库中删除一个标签，需要使用`git push <remoteRepository>: refs/tags/<tagName>`这一个命令。该命令有两种变体

   - `git push <remoteRepository> :refs/tags/<tagName>`是使用==:==号前面的空值代替原有的==tagName==，从而可以高效删除该标签

   - 另外还有一种更加直观的方式：

     ```bash
     $ git push <remoteRepository> --delete <tagName>
     ```

#### 检出标签

1. 可以使用`git checkout <tagName>`这一个命令来查看==tagName==指向的文件版本
2. `checkout`命令会使得仓库处于==头指针分离(detacthed HEAD)==的状态。在这种状态下，如果你做了某些修改然后提交了它们，标签不会发生变化，但是新提交的内容将不属于任何一个分支，并且将无法访问，除非是通过使用确切的提交时候的哈希值才能对其进行访问。
3. 在==头指针分离==的状态下，如果我们需要进行修改提交，通常我们应该新建一个分支。

### Git别名

1. Git不会在我们输入部分命令时自动推断出我们想要的命令，这也就意味着我们必须牢记每一个命令。

2. 但是有些命令很长，不仅难记，而且敲这些命令也需要大量时间。

3. 此时我们可以为这些命令起一个简短的别名，今后就可以使用这个别名代替其指代的命令。

4. `git config`命令可以修改==config==文件，而我们又可以在==config==文件中配置相应的Git命令别名，所以我们可以通过`git config`命令来给Git命令起别名。

5. 例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git config --global alias.cfg 'config --global'
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git cfg alias.cmmta 'commit -a -m'
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $
   ```

   - `$ git config --global alias.cfg 'config --global'`为`config --global`起了一个别名为`cfg`，今后就可以使用`cfg`代替`config --global`了
   - 然后使用`cfg`又给`commit -a -m`起了一个别名为`cmmta`。

6. 如果是需要为外部命令定义别名，就需要在外部命令之前加上一个==!==号。例如

   ```bash
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git cfg alias.md '!mkdir'
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ git md hahaha
   
   helloworld@surface MINGW64 ~/Desktop/gitLocalRepository (master)
   $ ls .
   hahaha/  test.txt  test03  tracked.txt
   ```

   就为外部命令`mkdir`定义了一个别名为==md==。

### 常用命令小结

#### 获取和添加仓库

1. 克隆一个现有仓库

   ```bash
   $ git clone <remoteRepository>
   ```

2. 初始化当前目录为本地仓库

   ```bash
   $ git init
   ```

3. 在本地库添加一个远程库

   ```bash
   $ git remote add <simpleName> <remoteRepository>
   $ git fetch <simpleName>
   $ git merge <simpleName/brunchNname>
   ```

   如果本地库当前分支设置了跟踪远程库的某一分支

   ```bash
   $ git remote add <simpleName> <remoteREpository>
   $ git pull <simpleName>
   ```

#### 使用Git进行文件的版本管理

1. 检查Git仓库目录中的文件状态

   - 查看文件状态

     ```bash
     $ git status
     ```

   - 以简洁的方式查看文件的状态

     ```bash 
     $ git status -s/--short
     ```

2. 将文件添加到暂存区

   ```bash
   $ git add <file/dir>
   ```

3. 将暂存区的文件一次性提交到本地库

   ```bash
   $ git commit [-m "<msg>"]
   ```

4. 对于已跟踪的文件，做修改之后可以跳过使用暂存区直接提交修改到本地库

   ```bash
   $ git commit -a [-m "<msg>"]
   ```

5. 查看已暂存和未暂存文件的修改（差异）

   - 查看暂存区和工作区中文件的差异

     ```bash
     $ git diff
     ```

   - 查看本地库和暂存区中文件的差异

     ```bash
     $ git diff --cached/--staged
     ```

6. 移除文件

   - 文件和本地库版本一致的文件

     - 同时从暂存区和工作目录中移除文件，此后版本管理中将不再有它

       ```bash
       $ git rm <fileName>
       ```

       

     - 仅将文件从暂存区中删除，工作区中的文件保留，其后可将其加入到忽略文件列表中不进行版本管理

       ```bash
       $ git rm --cached <fileName>
       ```

   - 对于和本地库版本不一致的文件的移除，均需要在上述命令中添加`-f`选项进行强制删除

     - 删除暂存区和本地库中的文件

       ```bash
       $ git rm -f <fileName>
       ```

     - 仅删除暂存区中的文件

       ```bash
       $ git rm -f <fileName>
       ```

7. 移动（重命名）文件

   ```bash
   $ git rm <from_file> <to_file>
   ```

   或者

   ```bash
   $ mv <from_file> <to_file>
   $ git rm <from_file>
   $ git add <to_file>
   ```

8. 查看提交记录

   ```bash
   $ git log
   ```

   其后可跟各种选项定制输出。

9. 忽略的文件的列表和规则在`.gitignore`文件中

#### 撤销操作

1. 修补提交

   ```bash
   $ git commit --amend [-m "<msg>"]
   ```

2. 取消暂存的文件

   ```bash
   $ git reset HEAD <file>
   ```

3. 撤销对文件的修改

   ```bash
   $ git checkout -- <file>
   ```

#### 远程仓库的使用

1. 查看远程仓库

   ```bash
   $ git remote
   ```

2. 添加远程仓库

   ```bash
   $ git remote add <simpleName> <remoteREpository>
   ```

3. 从远程仓库中抓取和拉取内容

   - 抓取内容

     ```bash
     $ git fetch <simpleName>
     ```

   - 拉取内容

     ```bash
     $ git pull <simpleName>
     ```

4. 推送到远程仓库

   ```bash
   $ git push <simpleName> <localBranch>
   ```

5. 查看某一个远程仓库信息

   ```bash
   $ git remote show <remoteRepository>
   ```

6. 远程仓库的重命名

   ```bash
   $ git remote rename <fromFile> <toFile>
   ```

7. 远程仓库的移除

   ```bash
   $ git remote rm/remove <remoteRepository>
   ```

#### 打标签

1. 查看完整标签列表

   ```bash
   $ git tag
   ```

2. 创建标签

   - 创建附注标签

     ```bash 
     $ git tag -a <tagName> [-m "<msg>"]
     ```

   - 创建轻量标签

     ```bash
     $ git tag <tagName>
     ```

   - 查看标签信息

     ```bash
     $ git show <tagName>
     ```

3. 补打标签

   ```bash 
   $ git tag [-a] <tagName> [-m "<msg>"] <checksum>
   ```

4. 共享标签

   - 共享一个标签

     ```bash
     $ git push <remoteRepository> <tagName>
     ```

   - 共享全部标签

     ```bash
     $ git push --tags
     ```

5. 删除标签

   - 从本地库删除一个标签

     ```bash
     $ git tag -d <tagName>
     ```

   - 删除远端服务器上的远程库的标签

     - 方式1

       ```bash
       $ git push <remoteRepository> :refs/tags/<tagName>
       ```

     - 方式2

       ```bash
       $ git push <remoteRepository> --delete <tagName>
       ```

6. 查看某个标签对应文件版本信息

   ```bash
   $ git checkout <tagName>
   ```

#### Git别名

1. 为Git内部命令起别名

   ```bash
   $ git config --global alias.<title> '<command>'
   ```

2. 为外部命令起别名

   ```bash
   $ git config --global alias.<title> '!<command>'
   ```

### END

## Git分支

### 分支简介

#### 提交对象(commit object)

Git在进行提交操作的时候，会保存一个提交对象，该提交对象包含的信息有：

1. 指向暂存内容==快照==的指针
   - 这里简单说一下==快照==这一概念（本人也不太懂）
   - **快照是数据存储的某一时刻的状态记录；备份则是数据存储的某一个时刻的副本**
   - ==快照==仅仅记录逻辑地址和物理地址的对应关系
   - ==备份==就是将物理数据做一次复制
   - 我们只要知道快照速度快很多，通常情况下占用的空间比备份少很多就行了。如果要研究清楚就得使用搜索引擎之类的了
2. 作者的姓名和电子邮箱
3. 提交时的提交说明
4. 指向它的父对象的指针。当然首次提交的提交对象没有父对象，其余的提交对象都有至少一个父对象

#### Git是如何保存数据的

1. 为了形象地说明Git是如何保存数据的，我们先假设我们现在有一个工作目录，里面包含三个将要被暂存和提交的文件；

2. 如果我们这时候执行**暂存操作**，那么暂存操作就会为每一个文件计算校验和，然后把当前版本的文件快照保存到Git仓库中（Git使用blob对象来保存它们），最后将校验和放入暂存区域中等待提交。如果我们对上面的三个文件都进行了暂存操作，此时Git仓库中就新增了三个blob对象；

3. 随后如果我们进行**提交操作**，这是Git会先计算每一个子目录的校验和（此时只有一个子目录就是根目录），其后将这些校验和以==树对象==的形式保存在Git中。然后再创建一个==提交对象==，提交对象包含上述的信息，**此外该对象还保存着一个指向刚才那个树对象的指针**。如此一来，Git就可以在需要的时候重现此次保存的快照

4. 最终可用下图表示初次提交后各个对象之间的关系

   ![image-20200527110126266](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527111955045.png)

   

5. 稍作修改之后再次提交,那么这次的提交对象就会包含一个指向上一次的提交对象（父对象）的指针，如下图所示

   ![image-20200527110152320](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527110126266.png)

#### 分支的实质

**Git的分支实质上就是指向提交对象的可变指针**。Git的默认分支是==master==,在多次提交之后，我们其实已经拥有了一个指向最后一个提交对象的==master==分支。

- **注** Git的==master==分支和其他的分支实质上没有任何区别，它也没有任何特殊的地方。之所以用的多，是因为`git init`会默认创建它，然后人们懒得改。

### 分支的创建

1. **注** 本文的图片大部分来自Git官方的中文教程中的图片

2. 在简单了解了分支的概念之后，我们就应该很想知道分支是怎么创建的。

3. 实际上这很简单，我们只需要使用`git branch <branchName>`命令就可以创建一个分支了。

4. 执行了上面的命令之后，

5. Git实际上就新建了一个可以移动的指向提交对象的指针。例如

   ```bash
   $ git branch testing
   ```

   这个命令就新建了一个`testing`的指针指向当前的提交对象（也就是最新的提交对象）。如下图所示

   ![image-20200527111955045](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527110152320.png)

6. 那么Git是如何知道当前在哪一个分支上呢？实际上在Git中还有一个名为`HEAD`的特殊指针，该指针指向当前所在的本地分支，如下图所示

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527112259747.png" alt="image-20200527112259747" style="zoom:50%;" />

7. **使用`git branch <branchName>`命令只是新建了一个分支，但是并不会自动切换到新建的分支中去**。也就是`HEAD`指针不会自动指向该新建的指针。

8. 我们可以使用`git log --decorate`命令查看当前的各个分支所指向的**提交对象**。

### 切换分支

1. 上面我们已经学习了如何新建一个分支，那么我们应该怎么样切换到这些已存在的分支上呢？

2. 我们需要使用`git checkout <branchName>`来切换到分支==testing==上。例如

   ```bash
   $ git checkout testing
   ```

   - 命令执行之后它就会提示我们现在已经切换到==testing==上了

   - 根据上面的讲述，我们应该知道了分支的切换，实际上就是`HEAD`指针改变了它所指向的对象。这里在执行该指令之后，`HEAD`就指向了`testing`。如下图所示

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527143912380.png" alt="image-20200527143912380" style="zoom:50%;" />

   

3. 然后我们在`testing`分支上做一些修改然后再提交

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527144947687.png" alt="image-20200527144037668" style="zoom:50%;" />

   - 如图所示，`testing`分支向前移动了，但是`master`分支却没有，依然停留在原地。

4. 然后我们切换回到`master`分支

   ```bash
   $ git checkout master
   ```

   - 此时，`HEAD`就会指向`master`。如下图所示

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527144037668.png" alt="image-20200527144947687" style="zoom:50%;" />

   - 这一条命令做了两件事：

     - 使`HEAD`指向`master`
     - 将工作目录中的文件内容恢复为`master`分支所指向的快照的内容

   - 也就是说，你现在切换回来的话，你就相当于回退到了一个较老的版本，它会忽略掉`testing`分支所做的修改

5. 从上面的描述来看，**分支的切换，是会改变我们工作目录中的内容的**。如果Git不能干净利落地改变工作目录中的内容，也就是在切换过程中如果可能使文件内容混淆错乱，这时候Git将会禁止分支的切换。换句话说，Git中进行分支的切换时可以保证数据的准确性的，否则Git会阻止那种会导致工作区数据内容错乱的切换。

6. 随后，我们又在`master`分支上进行了修改和提交，这时候，项目的提交历史就会产生分叉，如下图所示

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527150018922.png" alt="image-20200527150018922" style="zoom:67%;" />

7. 我们可以不断地在分支之间进行切换和工作，这些分支之间的提交互不干扰，因此在它们上进行的工作也互不干扰。在实际成熟之后，我们还可以把它们合并起来。

8. 使用`git log --oneline --decorate --graph --all`命令可以查看项目的整个提交历史，项目的分支分叉情况，以及各个分支的指向。

9. Git分支实质上是仅包含其所指对象的检验和（包含40个字符的SHA-1值）的文件，因此它的创建和销毁都异常高效。创建一个新分支就相当于在文件中写入41个字节。

10. 在创建分支的同时切换到创建的分支的命令如下

    ```bash
    git checkout -b <newBranchName>
    ```

    

### 分支的新建和合并

1. 首先我们在`master`分支上已经进行了一些工作，产生了一些提交，如下图

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527151729990.png" alt="image-20200527151729990" style="zoom:50%;" />

2. 然后可能想开发一个新功能，我们新建一个分支`iss53`来进行这个开发工作

   ```bash
   $ git checkout -b iss53
   ```

   - 结果如图

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527151910011.png" alt="image-20200527151910011" style="zoom:50%;" />

3. 然后我们在`iss53`分支上进行了一些工作并进行了一次提交，就变成如下这样

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527152025027.png" alt="image-20200527152025027" style="zoom:50%;" />

4. 这时候我们发现`master`上存在bug，那么这个时候，分支的优势就体现出来了，因为我们如果想修复bug，只需要将分支切换回`master`就可以将工作区中的内容恢复到当时的版本了，而不是需要将`iss53`分支上的修改撤销。

   ```bash
   $ git checkout master
   ```

   - **注** 我们在切换分支的时候务必确保当前所在的分支上的所有工作都已经进行了提交，否则会导致分支的切换失败

5. 然后我们创建一个新分支`hotfix`用于修复bug

   ```bash
   $ git branch hotfix
   $ git checkout hotfix
   ```

   - 随后在其上进行了一些工作，进行了一次提交，就变成下面这样

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527154229252.png" alt="image-20200527152853839" style="zoom:50%;" />

#### 分支的快进合并

1. 然后经过测试，我们发现，`C4`的修改把bug修复了，然后我们就可以将`hotfix`分支合并回到`master`上

   - 首先我们要将分支切换到我们想要并入的分支，本例中就是`master`分支

     ```bash
     $ git checkout master
     ```

   - 然后执行合并命令`git merge <branchName>`，此处的==branchName==是要合并进来的分支，本例中是`hotfix`分支

     ```bash
     $ git merge hotfix
     ```

   - 执行命令之后的输出信息如下

     ```bash
     $ git checkout master
     $ git merge hotfix
     Updating f42c576..3a0874c
     Fast-forward
     index.html | 2 ++
     1 file changed, 2 insertions(+)
     ```

2. 在上面的合并输出的时候，我们应该有注意到这样的一个词语==Fast-forward==。这个词语的含义是

   - 当我们试图合并两个分支的时候，如果顺着一个分支走下去能够到达另外一个分支，那么在合并这两个分支的时候，只会将落后的指针向前推进到另外一个指针的地方

   - 因为这样的合并没有需要解决的分歧，所以叫做==快进==

   - ==快进==合并之后的结果如图所示

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527154708589.png" alt="image-20200527154229252" style="zoom:50%;" />

3. 在完成了合并之后，我们就可以删除其中的某一个分支，例如本例中，要把`hotfix`分支删除，那么我们就可以通过`git branch -d <branchName>`命令进行删除操作。本例为

   ```bash
   $ git branch -d hotfix
   ```

   - 此时，分支的情况就变成了下图这样

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527154848656.png" alt="image-20200527154708589" style="zoom:80%;" />

4. 然后我们又切换回到`iss53`分支上继续进行开发，如下图所示

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527152853839.png" alt="image-20200527154848656" style="zoom:67%;" />

5. 这里需要注意的是，我们在`hotfix`分支上所做的修改并没有包含在`iss53`分支上。如果我们需要其中的修改数据，我们可以使用`git merge master`命令将`master`分支合并到`iss53`分支上。另外我们也可以等`iss53`分支的开发任务完成之后再把它合并回到`master`分支上。

#### 分支的分叉合并

1. 假设此时我们已经完成了开发，打算将`iss53`分支合并到`master`分支上，这时候和前面的合并操作没有什么分别

   - 切换到`master`分支

     ```bash
     $ git checkout master
     ```

   - 把`iss53`分支合并进来

     ```bash
     $ git merge iss53
     ```

   - 然后可以将多余的分支`iss53`删除

     ```bash
     $ git branch -d iss53
     ```

2. 但是这次合并的输出信息和和上次合并`hotfix`分支的情况有点不一样，如下所示

   ```bash
   $ git checkout master
   Switched to branch 'master'
   $ git merge iss53
   Merge made by the 'recursive' strategy.
   index.html | 1 +
   1 file changed, 1 insertion(+)
   ```

   - 此时的合并的信息是==Merge made by the ‘recursive’ strategy==，其中==recursive==的意思是递归、循环的意思
   - 这是因为本次合并的两个分支没有谁是谁的祖先，两个分支是分叉形成的

3. 为了对这样的两个分支进行合并，Git要做一些额外的工作：

   - 出现这样的情况的时候，Git会使用两个分支的末端所指的快照（==C4==和==C5==）以及两个分支的共同祖先（==C2==）来做一个==三方合并==。如下图所示

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527173114246.png" alt="image-20200527173114246" style="zoom:80%;" />

   - 和之前的快进合并不同的是，Git将这次的三方合并的结果做了一次新的快照，并自动创建一个新的提交对象指向这个新的快照。这个提交被称作==一个合并提交==，其特别之处在于，这样的提交对象有着不止一个父提交，如下图所示

     ![image-20200527173204349](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527173204349.png)

   - 从图中可以看到`C6`有着两个父提交，分别是`C4`和`C5`

#### 遇到冲突时的分支合并

1. 有时候我们的合并不会如此顺利。例如如果我们在两个分支中都对同一个文件的相同部分（比如同一行或者是相同的几行）分别进行了修改，这时候Git就无法干净地合并它们。

2. 对于这样的情况，Git会将他们合并起来但是不会自动生成一个新的提交。在合并完之后，Git会暂停下来，等待我们自己去解决这些冲突然后再自行提交。

3. 在发生合并冲突之后，我们可以使用`git status`查看是那些文件产生了冲突而处于未完成合并的状态。例如

   ```bash
   $ git status
   On branch master
   You have unmerged paths.
   (fix conflicts and run "git commit")
   Unmerged paths:
   (use "git add <file>..." to mark resolution)
   both modified: index.html
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   上面个就列出了未完成合并的文件名以及原因

   - ==Unmerged paths:==列表中的就是因为冲突而未完成合并的文件列表
   - ==both modified==是文件冲突的原因，两个分支都对同一个文件进行了修改

4. Git会将两个分支的同一个文件的冲突部分都合并到一个文件中，这个文件就是未完成合并的文件，冲突的内容使用特殊的标记标出，并标明哪一部分是哪一个分支的。

   - 我们可以分别打开这些未完成合并的文件，看到冲突的信息。例如

   ```text
   <<<<<<< HEAD:index.html
   <div id="footer">contact : email.support@github.com</div>
   =======
   <div id="footer">
   please contact us at support@github.com
   </div>
   >>>>>>> iss53:index.html
   ```

   - 冲突信息分析
     - ==<<<<<<< HEAD:index.html==这一行表示`HEAD`所指向的分支的文件版本（在本例中也就是`master`分支中的文件版本）中的冲突内容
     - ==>>>>>>> iss53:index.html==同理。表示`iss53`分支中的文件版本的冲突部分的内容
     - `======`是分隔符，上半部分是`HEAD`所指向的分支的文件中的冲突内容，下半部分是`iss53`分支的文件中的冲突内容

5. 以上具有冲突信息的文件需要我们手动修改合并。比如说对于重复的内容保留其中的一个，或者是对于不同的内容进行修正合并。修改完成之后要把上面标有着特殊含义的行都删除，就是上面冲突分析指出那几行。

6. 修改完成之后，需要使用`git add`命令将这些文件都进行暂存，`git add`之后，Git就会将它们标记为冲突已解决的状态

7. 随后还可以使用`git status`工具来确认是否所有冲突都已经解决。如果是，并且自己对于修改的结果都已经满意，就可以使用`git commit`命令来完成合并提交。

8. **只有使用`git commit `命令完成了合并提交，本次合并操作才算彻底完成了**。

### 分支管理

**本小节主要是学几个常用的分支管理的工具**

`git branch`命令不只是可以创建和删除分支。如果不加任何参数执行它，就会列出当前所有分支的列表，这跟`git tag`命令类似。

- `git branch`命令列出当前所有分支。例如

  ```bash
  $ git branch
  iss53
  * master
  testing
  ```

  - 其中带==*==号的分支就是当前所在的分支或者说是`HEAD`所指向的分支。

    

- `git branch -v`可以看到每一个分支的最后一次提交的信息。例如

  ```bash
  $ git branch -v
  iss53 93b412c fix javascript issue
  * master 7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
  ```

  - 我们可以看到每一个分支最后一次提交时的部分校验和和提交说明

- `git branch`后增加`--merged`和`--no-merged`选项可以过滤出当前所有的分支中已经合并到当前分支或者是没有合并到当前分支的分支列表

  - `--merged`选项通常用来查看哪些分支是已经合并到当前分支的。然后可以将这些多余的分支进行删除操作

  - `--no-merged`选项可以选出那些还没有合并到当前分支的分支。如果我们要对这些分支进行删除操作，如果使用的是`gti branch -d <branchName>`会删除失败，因为它们包含了还没有合并的工作。如果非要删除，可以增加`-D`选项来进行强制删除

  - 实际上这两个选项还可以在后面指定分支名，那么这时候过滤的就是已经合并到指定分支或者是还没有合并到指定分支的分支列表了。例如

    ```bash
    $ git checkout testing
    $ git branch --no-merged master
    topicA
    featureB
    ```

    就是说列出当前还没有合并到master分支的分支列表，然后==topicA==和==featureB==就是还没有合并到`master`分支的分支

  - 也就是说，如果其后有分支名，就过滤的是和指定分支是否合并的分支列表；如果没有，就默认这个指定的分支是当前所在的分支。

### 分支开发工作流

**这部分估计也就本人看得懂，大家看不懂的就凑个热闹好了，语言表达能力太贫乏，没文化真就只能一句 ‘wo曹’走天下了**

#### 长期分支

长期分支的工作方式大概可以描述为：用一个分支保留最稳定的代码，然后使用另外的分支将工作向前推进，工作进行到一定的阶段，代码变得稳定之后就并入稳定的分支，然后再转换到其他分支上继续推进工作，如此反复。这就相当于用一个分支用作版本发布，不在其上面工作，使用另外的分支推进工作。它的方式相当于下图

![image-20200527211334747](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527211334747.png)

- 这类似于流水线工作

  ![image-20200527211431973](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528091250777.png)

- 这就相当于多个分支在一线上跑，前面的是测试版，后面的是稳定版，前面的测试稳定了就将稳定版分支指针移到向前移动到当前位置，然后发布新的稳定版，然后测试版又向前推进。

#### 主题分支

主题分支是一种短期分支。主题分支就是按照主题进行分支，每一个分支的开发都有某一个单一目标和主题，当该分支达到了其目标功能或者是实现了它的主题，就将其合并回到主干分支。

- 就比如说，我想开发某一个功能，那我就创建一个分支来开发这个功能，这个分支的创建就是用来在其上完成该功能的开发。我一旦想开发一个新功能，我就创建一个新分支，然后再这个分支上的工作就围绕这个主题（功能）展开，而一旦完成了这个功能，就把这个分株合并回到主干分支。

#### 长期分支和主题分支的区别

1. 长期分支的意思大概就是说，一个项目长期保持着多个分支，然后这些不同的分支指向不同稳定性的阶段性工作成果，这些分支没有明显的主题，实际上，所有的功能都是在一条线上开发的，一个分支开发多个功能甚至是所有功能。分支的主要目的就是，用一些分支来开发新功能，开展新工作，而另外一些分支纯粹是为了保存稳定的版本内容，用作向公众发布。
2. 主题分支的一个显著的特点就是，按照主题来创建分支，每一个分支有着明确的主题和任务，任务完成后就合并到主干分支。没有新功能或者新主题的时候可能项目只有一个分支，只有加入新主题，才创建新分支，分支的任务完成之后就将其与主干分支合并然后将其删除。

### 远程分支

1. 我们上面的所有操作，所创建的全部分支都是存储在本地的，我们都没有和服务器发生交互

2. **远程引用**是对远程仓库的引用（指针），包括分支、标签等。

   - 我们可以通过`git ls-remote <remoteRepoditory>`来显示获得远程引用的完整列表。例如

     ```bash 
     $ git ls-remote con
     2fb972523cbca9fbf6cd3730b34fb38af552533d        HEAD
     2fb972523cbca9fbf6cd3730b34fb38af552533d        refs/heads/master
     ```

     

   - 或者是`git remote show <remoteRepository>`来获取远程分支的更多信息。例如

     ```bash 
     $ git remote show con
     * remote con
       Fetch URL: https://github.com/srevinsaju/conozco.git
       Push  URL: https://github.com/srevinsaju/conozco.git
       HEAD branch: master
       Remote branch:
         master new (next fetch will store in remotes/con)
       Local branch configured for 'git pull':
         master merges with remote master
       Local ref configured for 'git push':
         master pushes to master (up to date)
     ```

3. **远程跟踪分支**是远程分支状态的引用，它们是我们无法移动的本地指针。一旦我们进行网络通信，Git就会自动移动这样的指针，以精确反应远程仓库的状态。这样的分支记录了我们最后一次连接到远程服务器上的远程仓库时，远程库上的该分支所处的位置。

   - **远程跟踪分支**，顾名思义就是用来跟踪远程仓库上的分支位置，保存在本地上的指针。这个指针的特别之处就在于它不能认为移动，是在和相应的远程库连接后Git进行自动移动的，即将该指针在本地移动到和远程库相应的分支指针相同的位置。==相当于自动同步==

   - 这样的分支以`<remoteRepository>/<branchName>`的形式命名。如果我们向查看我们最后一次连接该远程库时该分支所在的位置，我们就可以以`<remoteRepository>/<branchName>`的形式查看该分支的信息。

   - 我们在对一个仓库进行克隆的时候，Git的`clone`命令会自动为我们创建一个表示该远程库的简写名`origin`，然后拉取它的所有数据，然后自动创建一个指向远程库`master`分支的指针`origin/master`，这就是一个远程跟踪分支。同时，Git还会为我们克隆的本地库创建一个本地分支`master`，它和`origin/master`指针指向相同的地方。`master`分支由我们自己控制，就是一个普通的本地分支，`origin/master`由Git自动控制，只有连接到相应的远程库并且远程库上的`master`分支移动了，该指针才会移动，并且会自动移动到和远程库的`master`分支当前所在位置一致的地方。

     <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200527211431973.png" alt="image-20200528091250777" style="zoom:80%;" />

     - `origin`这一个名称和`master`一样，没有什么特殊的含义，只是因为`clone`命令会默认生成这样的一个名称。

   - `<remoteRepository>/<branchName>`这个指针是保存在本地的，但是使用Git自动控制的，只有当本地和==remoteRepository==连接后，该指针的位置才会更新，它指向的位置总是最后一次和远程服务器连接之后相应的分支的最新位置。**只要不和远程服务器连接通信，他就不会变**

   - **小结** 上面所说的内容涉及了三个分支

     - 远程服务器上的对应远程库上的`master`分支，这一分支是位于远程服务器上的
     - 本地仓库中的`master`和`oringin/master`分支，这两个都是在本机上的
     - 位于本机上的两个分支，实际上都是指向本地仓库的提交历史。`master`分支是由我们自己掌控的用来进行工作的，而`origin/master`分支是指向本地历史的，但是是Git自动控制的，它用来追踪远程服务器上的`master`的状态，因此只有和远程服务器进行通信的时候它才能获得远程服务器上的`master`分支的最新位置，也才能进行更新。
     - 因此，远程服务器上的`master`分支的推进不会影响本地的`master`分支的工作，本地`master`分支的推进也不会改变`origin/master`分支的位置。一旦连接到远程服务器，`origin/master`就会和远程服务器上的`master`分支同步，断开连接之后，`origin/master`分支就会停在这样的位置不动，直到下一次联机同步。

4. **这里补充一下，上面所说的==远程跟踪分支==的自动更新并非真的不需要任何操作Git就会帮我们做好。实际上我们要将`origin/master`这样的远程跟踪分支进行更新，必须要手动执行`git fetch <remoteRepository>`命令，这时候Git才会帮我们自动抓取远程服务器上的`master`更新的内容到本地，然后自动移动`origin/master`指针与之同步。也就是说，如果我们不手动拉取远程库上的信息，Git是不会帮我们自动拉取这些更新的内容下来的。这也很好理解，因为我们连接到远程服务器不总是想要更新我们本地的数据的**

#### 推送

1. 当我们想要公开分享一个分支的时候，我们需要将其推送到有写入权限的远程仓库上。本地的分支并不会自动地和远程库进行同步，**我们必须显示推送我们想要分享的分支**。

2. 这样的好处就是我们可以自行决定我们想要分享和不想分享的分支。我们可以只把我们想要分享的分支推送到远程库上。

3. `git push <remoteRepository> <branchName>`命令就是用来推送分支到远程库上的

   - 在我们执行这样的一条命令之后,Git实际上会自动帮我们把==branchName==展开成`refs/heads/<branchName>:refs/heads/<<branchName>`。

   - 实际上我们也可以使用这样的一个命令来推送我们的分支

     ```bash
     $ git push <remoteRepository> <localBranchName>:<remoteBranchName>
     ```

     这样就可以把一个本地分支推送到远程库并可以给它起一个和本地分支名不同的名字

4. 在我们把分支推送到远程服务器上之后，别人就可以从服务器上抓取这一分支的内容到本地

   - 我们已经知道，`git fetch <remoteRepository>`命令只会把远程库的数据下载到本地，但是不会将这些数据自动合并到本地仓库

   - 所以，如果我们通过该命令抓取到了一个新的分支，它也不会自动合并到我们当前所在的分支，而是在本地多了一个`<remoteRepository>/<branchName>`指针，也就是远程跟踪分支

   - 我们可以将这一分支和某一个分支进行合并，这和上面讲述的本地库的分支的合并没有什么区别

     - 指定分支名，就把远程跟踪分支合并到指定的分支

       ```bash
       $ git merge <localBranch> <remoteRepository>/<branchName>
       ```

     - 不指定分支名，就将远程跟踪分支和当前分支进行合并

       ```bash
       $ git merge <remoteRepository>/<branchName>
       ```

   - 如果想要在一个自己本地的分支上工作，但是这些工作建立在远程跟踪分支之上，那么我们可以使用下面的命令

     ```bash
     $ git checkout -b <localBranch> <remoteRepository>/<branch>
     ```

     - 这一命令就会给我们一个用于工作的本地分支`<localBranch>`，分支的起点位于`<remoteRepository>/<branch>`
     - 这时候实际上我们就建立了一个==跟踪分支==`<localBranch>`

#### 跟踪分支

1. 从一个远程跟踪分支上检出（也就是执行的`git chechout`命令）一个本地分支会自动创建所谓的==**跟踪分支**==（它跟踪的分支叫做==**上游分支**==）。

2. **跟踪分支**是一个与远程分支有直接关系的本地分支。如果在一个跟踪分支上输入`git pull`，Git能自动识别要到哪一个服务器上去抓取数据，然后合并到哪一个分支

3. 在我们克隆一个仓库的时候，Git会自动创建一个跟踪`origin/master`分支的`master`分支

4. 我们也可以自行设置跟踪分支或者不跟踪哪一个分支，我们还可以设置一个在其他远程仓库上的跟踪分支

5. 设置跟踪分支的方法

   - `git checkout -b <branchA> <remoteRep>/<branchB>`
   - `git checkout --track <remoteRep>/<branch>`这个命令直接==创建==一个和远程分支同名的跟踪分支
   - 如果我们在检出一个分支的时候，该分支名不存在，而又刚好这时候在本地记录的所有远程库中只有一个远程库有一个分支叫这个名字，那么这个时候Git就会为我们自动==创建==一个叫这个名字的跟踪分支，跟踪的是那个唯一具有同名的远程分支。即`git checkout <branch>`

6. 上面设置跟踪分支的方法都是新建分支来进行跟踪的。如果想要使用一个已经存在的分支来跟踪刚刚抓取下来的远程分支，或者说是想要修正正在跟踪的上游分支，可以使用`-u`或者是`--set-upstream-to`选项运行`git branch`来显式设置。例如

   ```bash
   $ git branch -u origin/master
   ```

   命令就将当前分支设置为`origin/master`的跟踪分支

7. **上游快捷方式** 设置好跟踪分支之后，我们可以使用`@{upstream}` or `@{u}`来引用它的上游分支。比如说我们正处在`master`分支上，并且该分支正在跟踪`origin/master`分支我们可以使用

   ```bash
   $ git merge @{u}
   ```

   来取代

   ```bash 
   $4 git merge origin/master
   ```

8. 如果想查看设置的所有跟踪分支，可以使用`git branch`的`-vv`选项。这会将所有的本地分支列出来并且包含更多的信息，比如每一个分支正在跟踪哪一个远程分支，以及本地分支是领先、落后，还是都有。例如

   ```bash 
   $ git branch -vv
   iss53 7e424c3 [origin/iss53: ahead 2] forgot the brackets
   master 1ae2a45 [origin/master] deploying index fix
   * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this
   should do it
   testing 5ea463a trying something new
   ```

   - 这里可以看到`iss53`分支正在跟踪`origin/iss53`分支，==ahead==的意思是领先，==2==代表领先的版本数。这意味着本地还有两个提交没有推送到远程服务器上

   - 也能看到`master`分支正在跟踪`origin/master`分支并且二者是同步的

   - 接下来还看到`serverfix`分支正在跟踪`origin/server-fix-good`分支，并且领先3落后1，==behind==就是落后的意思，其后的==1==就是落后的版本数。这意味着本地有3次提交没有推送到远程服务器上，远程服务器上有一次提交没有被并入本地分支

   - 最后看到`testing`没有跟踪任何分支

   - ==**注意**== **这里的领先和落后的次数都是拿本地的跟踪分支和其跟踪的远程跟踪分支进行比较的，也就是比较的是本地跟踪分支和最后一次连接远程库时更新的远程跟踪分支的数据**

   - 想要统计最新的领先和落后次数，必须在查看之前先对所有的远程分支进行抓取。可以使用下面的命令

     ```bash
     $ git fetch --all
     ```

#### 拉取

1. 使用`git fetch`命令会把服务器上有而本地没有的数据抓取到本地，但是这些数据不会自动合并到本地的跟踪分支上，而是保存在远程仓库的本地引用中，我们需要使用`git merge`进行手动合并
2. 如果我们已经设置好了相应的跟踪分支，那么此时就可以使用`git pull`自动抓取服务器上的数据，然后Git会自动将这些抓取到的内容找到相对应的本地的跟踪分支来进行合并。
3. 通常建议使用`git fetch`和`git merge`而不是直接使用`git pull`

#### 删除远程分支

1. 如果我们已经通过远程分支完成了所有的工作，并且将远程分支的内容和服务器上的`master`分支（或者是其他的分支）进行了合并。**也就是说在服务器上的某一个分支的工作已经完成并且已经和服务器上的`master`分支（或者其他分支）进行了合并，那么该分支相当于多余的可删除的分支。**那么我们就可以将它删除了。

2. 删除一个远程分支的命令是

   ```bash
   $ git push <remoteRep> --delete <RemoteBranch>
   ```

   - 该命令会将==RemoteBranch==从远程服务器上删除

3. 实际上上面的删除命令只是将服务器上的这个代表某一个分支的指针删除了，而分支上的数据Git通常会将其保留在服务器上一段时间，直到垃圾回收运行。所以如果一不小心删除了一个远程分支，通常是很容易就可以恢复的。

### 变基

在Git中整合来自不同分支的修改主要有两种方法：`merge`和`rebase`。本节主要就是==`rebase`（变基）==

#### 变基的基本操作

1. 回顾前面的**分支的合并**的方式，对于如下的两个分支

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528203202339.png" alt="image-20200528203202339" style="zoom:50%;" />

2. 使用`merge`命令将两个分支合并。合并的过程可以描述为：Git会把两个分支的最新快照（==C4==和==C3==）以及二者的最近的共同祖先（==C2==）进行三方合并，并且为合并的结果形成一个新的提交，如下图

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528205758154.png" alt="image-20200528203540423" style="zoom:80%;" />

3. 这里介绍一种新的合并方法——**变基**：我们可以提取在==C4==中引入的修改和补丁，然后在==C3==的基础上再应用一次==C4==的修改和补丁。这样的操作再Git中就被成为**变基**。

4. 我们可以使用`rebase`命令将提交到==某一个分支==上的所有修改都转移到另外一个分支上，就好像在==另外一个分支==上面**重新播放**一遍这个==某一个分支==上面的所有修改一样。

5. 对于上面的例子，我们可以`checkout`==experiment==分支，然后把它变基到==master==分支上。这只需要执行如下的命令

   ```bash
   $ git checkout experiment
   $ git rebase master
   First, rewinding head to replay your work on top of it...
   Applying: added staged command
   ```

   - 其原理是首先找到这两个分支（当前分支==experiment==、变基操作的目标基底分支==master==）的最近的共同祖先`C2`

   - 然后对比当前分支==experiment==相对于该祖先的历次提交，然后提取相应的修改并保存为临时的文件。也就是说，找到最近的共同祖先之后，使用一个文件保存下来当前分支==experiment==的历次提交的修改内容。

   - 然后将当前分支==experiment==指向目标基底分支的最新一次的提交`C3`，最后将上面临时保存的文件中的历次修改依次应用。这就相当于在==master==分支上在其最新的提交之后依次重新提交一遍==experiment==上自与==master==最近共同祖先之后的历次修改。过程如下图所示

     ![image-20200528205758154](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528203540423.png)

   - 然后，我们还需要回到==master==分支上执行一次**快进合并**

     ```bash
     $ git checkout master
     $ git merge experiment
     ```

   - 这就有点类似于==长期分支==的流水形式。也就是将另外一个分支所做的工作搬到想要并入的分支的最前面，然后再将==master==指针向前移动。如下图所示

     ![image-20200528210210293](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528210210293.png)

6. 执行变基之后，**Figure 38**中`C4'`指向的快照实际上和**Figure 36**中的`C5`指向的快照是一模一样的。这两种合并的方法的最终合并结果并没有什么差别。但是**变基**使得提交历史变得更加整洁。

7. 当我们查看经过一个**变基**之后的分支的提交历史时我们会发现，尽管实际的开发工作是并行的，但它们看上去就好像是串行的一样，提交的历史是一条直线而没有分叉。也就是说，经过变基之后的分支的提交历史不再是实际的像**Figure 36**中的样子，而是像**Figure 38**中那样，把另外一个分支的提交历史接在了==并入到==的分支的提交历史的前面。

8. 我们使用**变基**的目的一般是为了确保在向远程分支推送时能保持提交历史的整洁。例如我们要为远程服务器中的某一个项目贡献代码，我们现在我们的本地分支上进行修改，然后再把这些修改变基到远程的某一个分支上，这样，远程仓库的代码维护者就不用进行整合工作，只需要快进合并即可。

9. **注意** 无论是三方合并还是变基，合并的最终结果所指向的快照是一样的，只不过是提交历史不同而已。变基是把一个分支的提交历史依次应用到另一个分支，而三方合并是将两个分支的最终结果合在一起。变基的提交历史是一条直线，三方合并则会有分叉。

#### 更有趣的变基例子

这个例子实际上可不是为了有趣而已。

1. 对两个分支进行变基时，所生成的**重放**并不一定要在目标分支上应用，也可以应用到另外一条分支上。

2. 例如下面的例子，一个项目的各个分支的提交历史如下

   ![image-20200528213448004](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528213448004.png)

3. 假设此时我们想要将`client`中的修改合并到`master`中并发布，但是暂时并不想合并`server`中的修改，可能这个分支的工作还没有完成。

4. 这时候我们就可以使用`git rebase`命令的`--onto`选项，选择那些在`client`分支中但是不在`server`分支中的修改（也就是`C8`和`C9`），将它们在`master`上重放，即

   ```bash
   $ git rebase --onto master server client
   ```

   - 我们可以看到该命令有三个参数

   - `master`为进行进行重放的分支（==第一个参数==）

   - `server`相当于两个分支在进行变基时的基底分支（==第二个参数==）

   - `client`分支就相当于进行变基的两个分支中的当前分支，是用来提取修改和补丁的（==第三个参数==）

   - 这一个命令的意思就是：取出在`client`上的，和`server`分叉之后的补丁，然后将这些补丁在`master`上重放一遍，让`client`看起来像是直接基于`master`进行了修改一样

   - 其结果如下图

     ![image-20200528214749314](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528214749314.png)

5. 然后可以快进合并`master`分支了

   ```bash
   $ git checkout master
   $ git merge client
   ```

   ![image-20200528214909946](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200528214909946.png)

6. 这上面的例子就说明了在开头说的**“对两个分支进行变基时，所生成的**==**重放**==**并不一定要在目标分支上应用，也可以应用到另外一条分支上”**这句话的含义。上面的例子实际上是`server`分支和`client`分支进行变基，但是却将变基的重放应用在了`master`分支上。

7. 接下来我们决定把`server`分支也合并到`master`分支上，这个时候的变基实际上就和普通的变基没什么区别了。这里介绍一个更加便捷的命令，如下所示

   ```bash
   $ git rebase <baseBranch> <topicBranch>
   ```

   对于本例，就是

   ```bash
   $ git rebase master server
   ```

   该命令就省去了要先切换到`server`分支，然后提取其补丁变基到`master`分支上，这个命令执行之后，结果如下图所示

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529082914058.png" alt="image-20200529082200467" style="zoom:150%;" />

8. 接下来我们就可以进行快进合并了

   ```bash
   $ git checkout master
   $ git merge server
   ```

9. 至此`client`和`server`分支都已经合并到了`master`分支里面。接下来我们就可以删除这两个分支了，最终的提交历史就会变成下图所示的样子

   <img src="https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529082200467.png" alt="image-20200529082914058" style="zoom:150%;" />

#### 变基的风险

1. **变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但是实际上不同 的提交**

2. 如果我们将仓库上的内容推送到了远程服务器，而其他人又从这个远程库中拉取了这些提交并进行后续工作，但是之后你使用`rebase`命令重新进行了整理再次提交推送，那么这时候，其他人就不得不重新拉取你的推送和他们正在进行的工作进行整合。如果接下来我们又拉取并整合了他们所做的提交，这时候，事情就会变得一团糟。

3. 我们用一个例子来说明这件事

   - 假如我们从一个服务器上克隆了一个仓库进行开发。提交历史如下图

     ![image-20200529083839219](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529083839219.png)

   - 随后有某个人向服务器推送了自己的修改，这其中包括一次合并

     ![image-20200529084029763](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529084029763.png)

   - 然后你把这些提交的内容拉取下来并和本地仓库进行了合并，我们本地库的提交历史就会变成下面这样

     ![image-20200529084150656](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529084150656.png)

   - 接下来这个人决定把刚才的合并撤销，采用变基的方式重新合并，然后使用`git push --force`命令重新推送到服务器上覆盖了上一次合并的提交历史，这时候服务器上的历史就变成了这样

     ![image-20200529084418401](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529084418401.png)

   - 然后我们又从服务器拉取更新，这时候我们发现多出来了一些新的提交

     ![image-20200529084612131](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529084612131.png)

   - 这就使得我们双方的处境都变得很尬尴。如果我们执行`git pull`命令，结果就像下图这样

     ![image-20200529085020207](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529085020207.png)

   - 这个时候就相当于我们又把相同的内容进行了依次合并，生成了一个新的提交。此时如果我们执行`git log`会发现，这两个提交的的作者，日期和日志都是一样的，这就会令人感到混乱

   - 如果我们也推送我们的内容到服务器，那么那些由于变基而被抛弃的提交又变回来了，这更加使人感到混乱。

#### 用变基解决变基

**用魔法打败魔法？太强了吧。老爹说的果然没有错**

1. 实际上，Git除了对整个提交计算SHA-1校验和之外，也会对本次提交所引入的修改计算校验和，即==patch-id==

2. 如果我们抓取被覆盖过的更新并将我们手头的工作基于此进行**变基**的话，Git一般都可以分辨出哪些是我们的修改，并把它们应用到新的分支上。

3. 例如，对于上面的例子，如果我们在抓取了新的推送数据后不是使用三方合并，而是执行`git rebase teamone/master`的话，Git将会：

   - 步骤1：检查哪些提交是我们的分支上独有的（`C2`, `C4`, `C3`, `C6`, `C7`）
   - 步骤2：检查上一个步骤筛选出的提交结果中不是合并操作结果的提交（`C2`, `C4`, `C3`）
   - 步骤3：再检查上一步筛选出的提交结果中那些在对方进行覆盖更新时没有被纳入目标分支的提交（只有`C2`和`C3`，因为`C4`实际上就是`C4’`）
   - 步骤4：把步骤3筛选出的最终提交结果依次应用到`teamone/master`上

4. 最终就得到了==在一个被变基然后强制推送的分支上再次执行变基之后的结果==如下图所示

   ![image-20200529094029084](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200529094029084.png)

5. 要想上述的方案有效，前提是对方在进行变基时`C4`和`C4’`几乎是一样的。否则变基操作就无法识别它们是相同的。然后就会创建一个新的类似`C4`的补丁，而这个补丁的内容又和`C4’`发生冲突，从而无法整洁地整合入历史

6. 对于本例的另外一个简单的方法是我们在拉取更新时，使用`git pull --rebase`而非直接`git pull`。又或者是先`git fetch`然后再`git rebase teamone/master`

7. 如果我们习惯使用`git pull`，但是有希望默认使用`--rebase`选项。我们可以执行下面这条语句来更改`pull.rebase`的默认配置

   ```bash
   $ git config --global pull.rebase true
   ```

   

8. 总之，我们要记住这样的一条原则：**如果我们的提交会存在于我们的本地仓库之外，同时别人又可能会基于这些提交做开发，那么我们这个时候就不应该使用变基**。

9. 实际上就是说，不应该出现上面例子中的那种情况，先推送了提交，之后又回滚，变基之后又重新强制推送。我们可以在我们的本地库中在做完工作之后先执行完变基，然后再推送到远程服务器上，这样就不会给别人造成混乱

#### 变基 vs. 合并

是使用变基还是使用合并，这似乎是一个哲学问题

1. 支持使用**合并而不是变基**的人认为：仓库的提交历史就应该如是记录开发的每一步，它就像历史一样，可以为以后的开发提供参考，而篡改历史是一种亵渎。所以即使是我们的提交历史是一团糟，也应该如实记录下来，以供后人查阅。
2. 而支持使用**变基而不是合并**的人则认为：仓库的提交历史应该像一本故事书，它可以清晰地告诉人们这个项目的开发故事。因此提交的历史应该是逻辑清晰的，就像是故事书一样，人们通常才不关注我们写这本书的时候的草稿，而我们也不应该将一篇草稿发布给别人看，而是应该仅仅向公众发布我们的修订好的版本。
3. 所以，是使用合并还是使用变基，没有一个强制性的规定，我们应该结合实际情况进行明智地选择。我们只要记住使用变基时候应该遵循的原则就不会错误地使用变基，在那种既可以使用变基也可以使用合并的场合，选用哪种方式，这就取决于个人了，取决于你的处世态度了。

### 本章小结

#### 分支的创建

1. 创建一个新分支

   ```bash
   $ git branch <branchName>
   ```

   

2. 查看各个分支指向的提交对象

   ```bash
   $ git log --decorate
   ```

   

#### 切换分支

1. 切换到一个分支

   ```bash
   $ git checkout <branchName>
   ```

   

2. 查看项目的整个提交历史，项目的分支分叉情况，以及各个分支的指向

   ```bash
   $ git log --oneline --decorate --graph --all
   ```

   

3. 新建分支的同时切换到该分支

   ```bash
   $ git checkout -b <branchName>
   ```

   

#### 分支的新建与合并

1. 分支的合并

   ```bash
   $ git merge [<A>] <B>
   ```

   把==B==并入==A==，==A==可以省略，省略就默认并入当前分支

   

2. 分支的删除

   ```bash
   $ git branch -d <branchName>
   ```

3. 遇到冲突时候的合并

   - 合并的时候遇到冲突

     ```bash
     $ git merge [<A>] <B>
     ```

     

   - 查看所有冲突文件

     ```bash
     $ git status
     ```

     

   - 修改冲突文件并加入暂存区标记为已解决冲突状态

     ```bash
     $ git add <file>
     ```

     

   - 提交修改后的冲突文件完成合并

     ```bash
     $ git commit
     ```

     

#### 分支管理

1. 列出当前所有分支

   ```bash
   $ git branch
   ```

   

2. 查看当前所有分支的最后一次提交的信息

   ```bash
   $ git branch -v
   ```

   

3. 分支的过滤

   - 过滤出那些已经和指定分支合并过的分支

     ```bash
     $ git branch --merged [<branchName>]
     ```

     - 如果省略后面的分支名，就是过滤出已经和当前分支合并过的分支
     - 否则就是过滤出已经和==branchName==分支合并过的分支
     - 这些分支可以使用`git branch -d <branchName>`命令进行删除

   - 过滤那些还没有和指定分支合并过的分支

     ```bash
     $ git branch --no-merged [<branchName]
     ```

     - 和`--merged`类似，过滤的是还没有和指定分支合并的分支
     - 这些过滤出来的分支要使用`-D`选项才能进行强制的删除

#### 远程分支

1. 获取远程引用的完整列表

   ```bash
   $ git ls-remote <remoteRep>
   ```

   

2. 获取远程分支的更多信息

   ```bash
   $ git remote show <remoteRep>
   ```

   

3. 更新远程跟踪分支

   ```bash
   $ git fetch <remoteRep>
   ```

   

4. 推送一个分支到远程仓库上

   ```bash
   $ git push <remoteRep> <localBranch>[:<remoteBranch>]
   ```

   - 后面的第二个参数为远程分支名，省略的话就和本地分支同名

5. 抓取远程库的内容到本地

   ```bash
   $ git fetch <remoteRep>
   ```

   

6. 将远程跟踪分支的内容和本地分支合并

   ```bash
   $ git merge [<localBranch>] <remoteRep>/<remoteBranch>
   ```

   - 如果不指定本地分支名，就默认为当前分支

7. 创建一个本地分支，并且其起点在和一个远程跟踪分支相同

   ```bash
   $ git checkout -b <newBranch> <remoteRep>/<branch>
   ```

   

8. 设置一个跟踪分支

   - 创建一个分支跟踪远程分支

     ```bash
     $ git checkout -b <brancha> <remoteRep>/<branchb>
     ```

     

   - 创建一个和远程分支同名的跟踪分支

     ```bash
     $ git checkout --track <remoteRep>/<branch>
     ```

     

   - 方式3

     ```bash
     $ git checkout <branch>
     ```

     - 如果==branch==不存在，但是远程引用中有唯一一个叫做==branch==的分支，那么该命令就会创建一个跟踪分支==branch==且它的上游分支就是那个唯一同名的远程分支

9. 为一个已经存在的分支设置上游分支使其成为一个跟踪分支

   ```bash
   $ git branch -u/--set-upstream-to <remoteRep>/<branch>
   ```

   

10. 访问跟踪分支的上游分支的快捷方式

    - 设置好跟踪分支后，可以使用`@{u}`或者是`@{upstream}`来引用上游分支。例如

      ```bash
      $ git merge @{u}
      ```

      

    - 就相当于

      ```bash
      $ git merge <remote>/<branch>
      ```

      其中`<remote>/<branch>`就是当前分支的上游分支

11. 查看设置的所有跟踪分支的信息

    ```bash
    $ git branch -vv
    ```

    

12. 抓取所有的分支数据

    ```bash
    $ git fetch --all
    ```

    

13. 拉取

    ```bash
    $ git pull <remoteRep>/<branch>
    ```

    

14. 删除一个远程分支

    ```bash
    $ git push <remoteRep> --delete <remoteBranch>
    ```

    

#### 变基

1. 将一个分支变基到指定分支

   ```bash
   $ git rebase <branchA> [<branchB>]
   ```

   - 省略==branchB==就默认将当前分支变基到==branchA==分支上
   - 否则就是将==branchB==分支变基到==branchA==上

2. 变基之后进行快进合并

   ```bash
   $ git merge <baseBranch>
   ```

   

3. 将两个分支的变基的**重放**应用到另外一个分支上

   ```bash
   $ git rebase --onto <branchA> <branchB> <branchC>
   ```

   - 将==branchB==和==branchC==两个分支的重放应用到==branchA==上
   - 具体是将==branchB==和==branchC==分叉后属于==branchC==而不属于==branchB==的提交历史在==branchA==上依次重放

4. 用变基解决变基

   ```bash
   $ git rebase <remoteRep>/<branch>
   ```

   - 这个命令最终会将在本地库独有的那些提交历史应用到`<remoteRep>/<branch>`上

### END

## Git工具

### 选择修订版本

Git能够以多种方式来指定单个提交、一组提交或者是一定范围内的提交。

#### 单个修订版本

可以通过任意一个提交时生成的40个字符的完整SHA-1散列值来指定一个提交。

#### 简短的SHA-1

通常，Git中，使用一部分的SHA-1散列值就能够唯一确定一个提交记录，我们在`git log`中加上`--abbrev-commit`选项就可以为每一个提交显示简短的唯一散列值。例如

```bash
$ git log --abbrev-commit --pretty=oneline
67520cb (HEAD -> master) 新建一个hello.c文件
```

#### 分支引用

1. 我们还可以直接使用分支名来指定一个提交记录，这时候，分支名指定的记录就是分支指针当前指向的最新提交记录。例如

   ```bash
   $ git show master
   commit 67520cb3fac87c26c2eb11b501c151c993b95c30 (HEAD -> master)
   Author: Square John <1042009398@qq.com>
   Date:   Sat May 30 16:37:20 2020 +0800
   
       新建一个hello.c文件
   
   diff --git a/.gitignore b/.gitignore
   new file mode 100644
   index 0000000..3110669
   --- /dev/null
   +++ b/.gitignore
   @@ -0,0 +1 @@
   +a.exe
   \ No newline at end of file
   diff --git a/hello.c b/hello.c
   new file mode 100644
   index 0000000..852836c
   --- /dev/null
   +++ b/hello.c
   @@ -0,0 +1,7 @@
   +#include <stdio.h>
   +
   +int main(void){
   +
   +       printf("Hello World\n");
   +       return 0;
   +}
   ```

   

2. 我们还可以使用`rev-parse`工具获取一个分支当前所指向的提交记录的SHA-1值。例如

   ```bash
   $ git rev-parse master
   67520cb3fac87c26c2eb11b501c151c993b95c30
   ```

#### 引用日志

1. 当我们在工作时，Git会在后台保存一个**引用日志**（==reflog==），引用日志保存了我们近几个月来的==HAED==和分支指针所指向的历史。因此我们可以使用`git reflog`来查看引用日志，例如

   ```bash
   $ git reflog
   67520cb (HEAD -> master) HEAD@{0}: commit (initial): 新建一个hello.c文件
   ```

   

2. 每当我们的==HEAD==指针的指向发生了变化，Git就会将这个信息存储在**引用日志**这个历史记录里。我们可以通过**引用日志**数据来获取之前的提交历史。

3. 如果我们想要查看HEAD最近的5次之前所指向的提交记录（也就是倒着往回数第6次的提交记录，我们可以使用`@{5}`来引用`reflog`中输出的提交记录。例如

   ```bash
   $ git reflog
   a5d2b56 (HEAD -> master) HEAD@{0}: commit: 添加一行'Hello C'
   b1fc8a9 HEAD@{1}: commit: 将换行符作为单独一行输出
   67520cb HEAD@{2}: commit (initial): 新建一个hello.c文件
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git show HEAD@{1}
   commit b1fc8a98292d9a97d786732d3fa575475348f5f9
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 09:44:57 2020 +0800
   
       将换行符作为单独一行输出
   
   diff --git a/hello.c b/hello.c
   index 852836c..e04f5f2 100644
   --- a/hello.c
   +++ b/hello.c
   @@ -2,6 +2,7 @@
   
    int main(void){
   
   -       printf("Hello World\n");
   +       printf("Hello World");
   +       printf("\n);
           return 0;
    }
   ```

   - 该命令实际上就是输出`reflog`日志中的相应的第`HEAD@{n}`次记录

4. 我们同样可以使用这样的语法来查看某一个分支在指定时间前的位置。例如

   ```bash
   $ git show master@{yesterday}
   warning: log for 'master' only goes back to Sat, 30 May 2020 16:37:20 +0800
   commit 67520cb3fac87c26c2eb11b501c151c993b95c30
   Author: Square John <1042009398@qq.com>
   Date:   Sat May 30 16:37:20 2020 +0800
   
       新建一个hello.c文件
   
   diff --git a/.gitignore b/.gitignore
   new file mode 100644
   index 0000000..3110669
   --- /dev/null
   +++ b/.gitignore
   @@ -0,0 +1 @@
   +a.exe
   \ No newline at end of file
   diff --git a/hello.c b/hello.c
   new file mode 100644
   index 0000000..852836c
   --- /dev/null
   +++ b/hello.c
   @@ -0,0 +1,7 @@
   +#include <stdio.h>
   +
   +int main(void){
   +
   +       printf("Hello World\n");
   +       return 0;
   +}
   ```

   

5. 可以使用`git log -g`来查看类似于`git log`输出格式的**引用日志**信息。例如

   ```bash
   $ git log -g master
   commit a5d2b563e6ffa56ef36383d4a1eb7bb5b690d1a1 (HEAD -> master)
   Reflog: master@{0} (Square John <1042009398@qq.com>)
   Reflog message: commit: 添加一行'Hello C'
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 09:49:11 2020 +0800
   
       添加一行'Hello C'
   
   commit b1fc8a98292d9a97d786732d3fa575475348f5f9
   Reflog: master@{1} (Square John <1042009398@qq.com>)
   Reflog message: commit: 将换行符作为单独一行输出
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 09:44:57 2020 +0800
   
       将换行符作为单独一行输出
   
   commit 67520cb3fac87c26c2eb11b501c151c993b95c30
   Reflog: master@{2} (Square John <1042009398@qq.com>)
   Reflog message: commit (initial): 新建一个hello.c文件
   Author: Square John <1042009398@qq.com>
   Date:   Sat May 30 16:37:20 2020 +0800
   
       新建一个hello.c文件
   ```

   - 如果不指定后面的分支名，就默认为输出当前分支的**引用日志**信息

6. **注意**： 

   - **引用日志**只存在于本地仓库，它只是一个记录我们在自己的仓库里做过什么的日志
   - 其他人拷贝的仓库里的引用日志不会和我们的相同
   - 如果我们刚刚新克隆了一个仓库，那么我们克隆所得的本地库中的**引用日志**是空的，因为我们在这个仓库里面还没有任何操作
   - `git show HEAD@{two.month.ago}`这一条命令只有当我们的本地仓库至少已经存在了两个月才能够显示匹配的提交

#### 祖先引用

1. 祖先引用是另一种指明一个提交的方式。

2. 如果我们在引用的后面加上一个`^`（脱字符），Git就会将其解析为该引用的上一个提交。例如

   ```bash
   $ git log --oneline
   a5d2b56 (HEAD -> master) 添加一行'Hello C'
   b1fc8a9 将换行符作为单独一行输出
   67520cb 新建一个hello.c文件
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git show HEAD^
   commit b1fc8a98292d9a97d786732d3fa575475348f5f9
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 09:44:57 2020 +0800
   
       将换行符作为单独一行输出
   
   diff --git a/hello.c b/hello.c
   index 852836c..e04f5f2 100644
   --- a/hello.c
   +++ b/hello.c
   @@ -2,6 +2,7 @@
   
    int main(void){
   
   -       printf("Hello World\n");
   +       printf("Hello World");
   +       printf("\n);
           return 0;
    }
   ```

   - 结果就是==b1fc8a9 将换行符作为单独一行输出==这一个提交记录

3. 在==Windows==的`cmd`中`^`是一个特殊字符，应该这样做

   ```bash
   $ git show HEAD^ #error
   $ git show "HEAD^" #ok
   $ git show HEAD^^ #ok
   ```

   

4. 我们还可以在`^`后面跟上一个数字指明我们想要的父提交。但是这种做法**只适用于有多个父提交合并提交**。例如

   ```bash
   $ git log --oneline --graph --all
   *   25dd4cc (HEAD -> master) Merge branch 'newbranch'
   |\
   | * 35241b9 (newbranch) 为readme文件新增了一行
   * | 14e9ff1 删除了打印hello world这一行
   |/
   * 2b3775c 修改了hello.c文件中的换行符位置错误
   * 62ed47d 新增一个readme文件
   * a5d2b56 添加一行'Hello C'
   * b1fc8a9 将换行符作为单独一行输出
   * 67520cb 新建一个hello.c文件
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git show HEAD^2
   commit 35241b91e2957c7dc9c22b47f0c394d473215c3a (newbranch)
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 10:43:26 2020 +0800
   
       为readme文件新增了一行
   
   diff --git a/readme b/readme
   index b34569e..6638245 100644
   --- a/readme
   +++ b/readme
   @@ -1 +1,2 @@
    这是一个说明文档
   +用于说明一些问题
   ```

   - `HEAD^2`就是`newbranch`中的==35241b9==这一个提交记录，也就是`HEAD`的第二个父提交

5. 另一种指明祖先提交的方法是在引用的后面加一个`~`号，`HEAD~`和`HEAD^`一样，都是指向`HEAD`的第一个父提交。但是`HEAD~2`表示的却是`HEAD`的第一个父提交的第一个父提交。对于`HEAD~3`，那就是`HEAD`的第一个父提交的第一个父提交的父提交，其余以此类推。其实`HEAD~3`也可以这样表示`HEAD~~~`

6. `^`和`~`还可以进行组合。例如`HEAD~2^2`就表示`HEAD`的第一个父提交的第一个父提交的第二个父提交

#### 提交区间

##### 双点

1. 最常用的指明提交区间的语法是双点，这种语法可以选出在一个分支中但是不在另一个分支中的提交历史

2. 例如

   ```bash
   $ git log --graph --oneline --all
   * 7c3bc83 (HEAD -> master) 为hello.c添加了一段程序用于求两个整数的相加的结果
   *   25dd4cc Merge branch 'newbranch'
   |\
   * | 14e9ff1 删除了打印hello world这一行
   | | * 95302bd (newbranch) 在readme末尾新加了一行说明
   | |/
   | * 35241b9 为readme文件新增了一行
   |/
   * 2b3775c 修改了hello.c文件中的换行符位置错误
   * 62ed47d 新增一个readme文件
   * a5d2b56 添加一行'Hello C'
   * b1fc8a9 将换行符作为单独一行输出
   * 67520cb 新建一个hello.c文件
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git log --oneline master..newbranch
   95302bd (newbranch) 在readme末尾新加了一行说明
   ```

   - 这个命令选出了在`newbranch`分支中但是还没有合并到`master`分支中的提交历史记录
   - 这样的语法可以帮助我们即使了解分支的最新情况，以及将要合并的内容

3. 这个语法的另外一个常用的场景就是用于查看我们将要推送到远方的内容

   ```bash
   $ git log origin/master..HEAD
   ```

   - 这个命令会输出在我们当前的分支但是不在`origin/master`分支的提交历史
   - 如果我们执行`git push`命令，并且我们当前的分支正在跟踪`origin/master`分支，则上面的命令所输出的提交内容就是我们将要提交的内容
   - 如果我们留空了其的一边，Git会默认为`HEAD`，例如`git log origin/master..`，那么我们也会得到和上面一致的结果。Git会使用`HEAD`来代替留空的一边

##### 多点

1. ​	有时候我们需要两个以上的分支才能确定我们所需要的修订。比如查看哪些提交是被包含在某些分支中的一个，但是不在当前的分支上

2. Git有这样的语法，在任意引用前加上一个`^`或者是`--not`用来指明不包含在指定分支中的提交。例如

   ```bash
   $ git log --oneline --all --graph
   * 7c3bc83 (master) 为hello.c添加了一段程序用于求两个整数的相加的结果
   *   25dd4cc Merge branch 'newbranch'
   |\
   * | 14e9ff1 删除了打印hello world这一行
   | | * 95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   | |/
   | * 35241b9 为readme文件新增了一行
   |/
   * 2b3775c 修改了hello.c文件中的换行符位置错误
   * 62ed47d 新增一个readme文件
   * a5d2b56 添加一行'Hello C'
   * b1fc8a9 将换行符作为单独一行输出
   * 67520cb 新建一个hello.c文件
   
   helloworld@surface MINGW64 ~/Desktop/cpl (newbranch)
   $ git log --oneline master..newbranch
   95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   
   helloworld@surface MINGW64 ~/Desktop/cpl (newbranch)
   $ git log --oneline ^master newbranch
   95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   
   helloworld@surface MINGW64 ~/Desktop/cpl (newbranch)
   $ git log --oneline newbranch --not master
   95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   ```

   - 其中这三条命令是等价的

     ```bash
     $ git log --oneline master..newbranch
     $ git log --oneline ^master newbranch
     $ git log --oneline newbranch --not master
     ```

     - 列出的都是在`newbranch`中但是不在`master`中的提交历史

   - 但是注意，下面这两个命令是不一样的

     ```bash
     $ git log --oneline newbranch --not master
     $ git log --oneline --not master newbranch
     ```

     - 前者是指，包含在`newbranch`中但是不包含在`master`中的提交记录
     - 后者是不包含在`newbranch`以及`master`中的记录

   - 但是下面这两个命令是一样的

     ```bash
     $ git log --oneline ^master newbranch
     $ git lig --oneline newbranch ^master
     ```

     - 都是不在`master`中但是在`newbranch`中的提交记录

   - 也就是说，`--not`是对其后所有的分支的否定，就是其后的分支都不包含的提交记录。而`^`只是对其后紧接着的一个分支的否定。

3. 这样的语法就可以这样使用

   ```bash
   $ git log --oneline ba bb ^bc
   ```

   - 这就表示，包含在`ba`和`bb`中但是不包含在`bc`中的提交记录

##### 三点

1. 这个语法可以可以选择出被两个引用之一包含但是又不同时被二者包含的提交历史

2. 例子如下

   ```bash
   $ git log --oneline master...newbranch
   7c3bc83 (master) 为hello.c添加了一段程序用于求两个整数的相加的结果
   95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   25dd4cc Merge branch 'newbranch'
   14e9ff1 删除了打印hello world这一行
   ```

   - 该命令就列出了所有只在`master`或者是`newbranch`分支中的其中一个的所有的提交记录

3. 在该命令中添加`--left-right`选项能够显示每一个提交所属的分支，这会让输出数据更加清晰。例如

   ```bash
   $ git log --oneline --graph --all
   * 7c3bc83 (master) 为hello.c添加了一段程序用于求两个整数的相加的结果
   *   25dd4cc Merge branch 'newbranch'
   |\
   * | 14e9ff1 删除了打印hello world这一行
   | | * 95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   | |/
   | * 35241b9 为readme文件新增了一行
   |/
   * 2b3775c 修改了hello.c文件中的换行符位置错误
   * 62ed47d 新增一个readme文件
   * a5d2b56 添加一行'Hello C'
   * b1fc8a9 将换行符作为单独一行输出
   * 67520cb 新建一个hello.c文件
   
   helloworld@surface MINGW64 ~/Desktop/cpl (newbranch)
   $ git log --oneline --left-right master...newbranch
   < 7c3bc83 (master) 为hello.c添加了一段程序用于求两个整数的相加的结果
   > 95302bd (HEAD -> newbranch) 在readme末尾新加了一行说明
   < 25dd4cc Merge branch 'newbranch'
   < 14e9ff1 删除了打印hello world这一行
   ```

### 交互式暂存

1. 本节的几个交互式Git可以帮助我们将文件的特定部分组合成提交。当我们修改了大量文件之后，希望将这些改动能够拆分为若干个单独的提交而不是混杂在一起成为一个提交时，这些工具特别有用。

2. 在运行`git add`时使用`-i`或者`--interactive`选项，Git会进入一个交互式终端模式。如下所示

   ```bash
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   hello.c
           modified:   readme
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           multipleAB.c
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git add -i
              staged     unstaged path
     1:    unchanged        +0/-3 hello.c
     2:    unchanged        +1/-0 readme
   
   *** Commands ***
     1: status       2: update       3: revert       4: add untracked
     5: patch        6: diff         7: quit         8: help
   What now>
   ```

   - 该命令会将已经进行了跟踪的文件列出来分为两列，其中一列为已暂存状态的内容的变化（就是对比本地库和暂存区中的对应文件差别），另外一列为为暂存状态的内容的变化（也就是对比工作区和暂存区中相应的文件的差异）
   - 然后我可以使用==Commdans==下面的命令交互式地对文件进行操作

#### 文件的暂存和取消暂存

1. 如果我们在==**What now>**==之后输入==**2**==或者是==**u**==或者是==**update**==，接下来就会出现如下界面

   ```bash
   What now> 2
              staged     unstaged path
     1:    unchanged        +0/-3 hello.c
     2:    unchanged        +1/-0 readme
   Update>>
   ```

   - 这时候它问我们需要暂存哪些文件

2. 加入我们输入如下，然后就会

   ```bash
   Update>> 1,2
              staged     unstaged path
   * 1:    unchanged        +0/-3 hello.c
   * 2:    unchanged        +1/-0 readme
   Update>>
   ```

   - 前面打上了==*****==号的文件表示将要被暂存的文件

3. 如果我们在==**Update>>**==之后什么也不输入而是直接回车，就会把前面带有==*****==号的文件进行暂存，接着界面就变成下面这样

   ```bash
   updated 2 paths
   
   *** Commands ***
     1: status       2: update       3: revert       4: add untracked
     5: patch        6: diff         7: quit         8: help
   What now>
   ```

   

4. 加入我们这个时候想要撤销对`readme`文件的暂存，我们可以输入==**3**==或者是==**r[evert]**==。如下所示

   ```bash
   What now> 3
              staged     unstaged path
     1:        +0/-3      nothing hello.c
     2:        +1/-0      nothing readme
   Revert>>
   ```

   

5. 这时候我们就可以选择要撤销暂存的文件

   ```bash
   Revert>> 2
              staged     unstaged path
     1:        +0/-3      nothing hello.c
   * 2:        +1/-0      nothing readme
   Revert>>
   ```

   - 此时前面带有==*****==号的文件表示将要撤销暂存的文件

6. 和前面一样，直接回车就可以将上面选择的文件撤销暂存

   ```bash
   Revert>>
   reverted 1 path
   
   *** Commands ***
     1: status       2: update       3: revert       4: add untracked
     5: patch        6: diff         7: quit         8: help
   What now>
   ```

   

7. 然后选择==**1**==或者是==**s[tatus]**==就可以查看这些已被追踪文件的状态

   ```bash
   What now> s
              staged     unstaged path
     1:        +0/-3      nothing hello.c
     2:    unchanged        +1/-0 readme
   
   *** Commands ***
     1: status       2: update       3: revert       4: add untracked
     5: patch        6: diff         7: quit         8: help
   What now>
   ```

   

8. 然后选择==**6**==或者是==**d[iff]**==就可以查看本地库中的文件和暂存区中相应的文件的差异，该命令和`git diff --cached`效果一样。首先是出现如下界面，让我们选择需要进行对比的文件

   ```bash
   What now> 6
              staged     unstaged path
     1:        +0/-3      nothing hello.c
   Review diff>>
   ```

   

9. 然后我们选择==**1**==，结果如下

   ```bash
   Review diff>> 1
   diff --git a/hello.c b/hello.c
   index 0384c93..1f84282 100644
   --- a/hello.c
   +++ b/hello.c
   @@ -3,8 +3,5 @@
    int main(void){
   
           printf("Hello C\n");
   -       int a = 10;
   -       int b = 20;
   -       printf("%d + %d = %d\n", a, b, a + b);
           return 0;
    }
   *** Commands ***
     1: status       2: update       3: revert       4: add untracked
     5: patch        6: diff         7: quit         8: help
   What now>
   ```

   

10. 然后选择==**7**==或者==**q[uit]**==退出交互式界面

#### 暂存补丁

1. 如果我们一个文件的多个地方进行了修改，但是我们只想要暂存其中的一个修改，那么这个Git也能轻松做到

2. 在上一节的交互式命令中，选择==**5**==或者是==**p[atch]**==，然后就会进入如下的选择界面

   ```bash
   What now> 5
   warning: LF will be replaced by CRLF in multipleAB.c.
   The file will have its original line endings in your working directory
              staged     unstaged path
     1:    unchanged        +5/-3 multipleAB.c
     2:    unchanged        +2/-0 readme
   Patch update>>
   ```

   

3. 然后选择相应的文件，如下所示

   ```bash
   Patch update>> 2
              staged     unstaged path
     1:    unchanged        +5/-3 multipleAB.c
   * 2:    unchanged        +2/-0 readme
   Patch update>>
   ```

4. 然后回车，进入如下界面

   ```bash
   Patch update>>
   diff --git a/readme b/readme
   index 6638245..7cbb288 100644
   --- a/readme
   +++ b/readme
   @@ -1,2 +1,4 @@
    这是一个说明文档
    用于说明一些问题
   +就是想添加一行文字
   +又添加一行文字
   (1/1) Stage this hunk [y,n,q,a,d,e,?]?
   ```

   

5. 然后输入==**?**==回车就可以看到各个选项的意义

   ```bash
   (1/1) Stage this hunk [y,n,q,a,d,e,?]? ?
   y - stage this hunk
   n - do not stage this hunk
   q - quit; do not stage this hunk or any of the remaining ones
   a - stage this hunk and all later hunks in the file
   d - do not stage this hunk or any of the later hunks in the file
   e - manually edit the current hunk
   ? - print help
   @@ -1,2 +1,4 @@
    这是一个说明文档
    用于说明一些问题
   +就是想添加一行文字
   +又添加一行文字
   (1/1) Stage this hunk [y,n,q,a,d,e,?]?
   ```

   

6. 然后我们可以选择==**y**==暂存这部分内容，随后就可以使用==**1**==来查看文件的状态

7. 实际上上面这个功能可以通过`git add --patch/-p`启动

### 贮藏和清理

1. 有时候我们可能会在某一个分支上进行了一些工作，做了一些修改，然后想要切换到另外一个分支进行一点工作，但是这个时候我们不想要将这些部分的还未完成的工作进行提交，我们可以使用`git stash`命令
2. **贮藏（stash）**会处理工作目录的脏的状态，即跟踪文件的修改和暂存文件的改动，然后将未完成的修改保存到一个栈上，随后我们还可以重新应用这些改动

#### 贮藏工作

1. 先在工作目录中修改几个文件，然后暂存其中的一个，使用`git status`查看当前仓库的状态

   ```bash
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           modified:   newfile
   
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   readme
   ```

   

2. 将新的修改推送到栈上贮藏，使用`git stash`或者是`git stash push`命令

   ```bash
   $ git stash
   Saved working directory and index state WIP on master: d414efe 新建了一个newile文件，并在其中输入了一行文字
   ```

   - 这时候会输出改动的内容

3. 然后我们在查看仓库的状态

   ```bash
   $ git status
   On branch master
   nothing to commit, working tree clean
   ```

   - 我们可以看到，仓库目录状态是干净的

4. 然后我们就可以切换到其他分支上工作了。而如果我们想要查看贮藏的东西，可以执行`git stash list`命令

   ```bash
   $ git stash list
   stash@{0}: WIP on master: d414efe 新建了一个newile文件，并在其中输入了一行文字
   ```

   

5. 使用`git stash apply`命令重新应用贮藏

   ```bash
   $ git stash apply
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   newfile
           modified:   readme
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   

6. 如果想要应用一个更旧版本的贮藏，可以使用它的名字指定它，例如`git stash apply stash@{2}`。如果不指定，就默认使用最新版的贮藏

7. 贮藏的内容并不一定要应用到进行贮藏的分支，也不要求应用贮藏时候的目录是干净的。**但是如果这时候修改的是刚刚才修改了的文件，这时候会产生合并冲突。**

8. 我们从上面的结果可以看到贮藏被重新应用了，但是刚才暂存的文件的内容被恢复了但是没有被重新暂存

9. 如果我们想要让刚才已经暂存的文件在重新应用贮藏之后也被重新暂存，我们可以在`git stash apply`之后加上`--index`选项

10. 应用了贮藏之后，堆栈上依然保留着这个贮藏的内容，我们可以使用`git stash drop <name>`删除指定贮藏内容，例如

    ```bash
    $ git stash list
    stash@{0}: WIP on master: 7244031 每一个文件增加了一行数字2
    stash@{1}: WIP on master: 92e51d0 每一个文件中写入了一行数字1
    stash@{2}: WIP on master: 1577b44 清空所有文件
    stash@{3}: WIP on tempbranch: 91d9942 不知道怎么回事的一个提交
    stash@{4}: WIP on master: d414efe 新建了一个newile文件，并在其中输入了一行文字
    
    helloworld@surface MINGW64 ~/Desktop/cpl (master)
    $ git stash drop stash@{4}
    Dropped stash@{4} (d9d901347cfaa02cebfb55d6fe207a631c3a2ed6)
    
    helloworld@surface MINGW64 ~/Desktop/cpl (master)
    $ git stash list
    stash@{0}: WIP on master: 7244031 每一个文件增加了一行数字2
    stash@{1}: WIP on master: 92e51d0 每一个文件中写入了一行数字1
    stash@{2}: WIP on master: 1577b44 清空所有文件
    stash@{3}: WIP on tempbranch: 91d9942 不知道怎么回事的一个提交
    ```

    

11. 我们还可以使用`git stash pop`命令在应用贮藏之后立即删除它

#### 贮藏的创意性使用

1. `git stash --keep-index`不仅将已暂存的文件主藏起来，还会将其保留在暂存区中。例如

   ```bash
   $ git stash --keep-index
   Saved working directory and index state WIP on master: 10c1009 为三个文件增加一行数字3
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           modified:   a.txt
   ```

   

2. 默认的`git stash`不会将新建的文件贮藏起来，只会贮藏哪些已经跟踪了的文件，对于还没有跟踪的文件是不会贮藏的。例如

   ```bash
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   a.txt
           modified:   newfile
           modified:   readme
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           b
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git stash
   Saved working directory and index state WIP on master: db19a07 为各个文件新增一行4
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git status
   On branch master
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           b
   
   nothing added to commit but untracked files present (use "git add" to track)
   ```

   

3. 可以使用`---include-untracked`或者是`-u`选项让Git的贮藏包含未跟踪的文件。当然这一个选项依然不会包含那些被忽略的文件。例如

   ```bash
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   a.txt
           modified:   newfile
           modified:   readme
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           b
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git stash -u
   warning: LF will be replaced by CRLF in b.
   The file will have its original line endings in your working directory
   Saved working directory and index state WIP on master: db19a07 为各个文件新增一行4
   
   helloworld@surface MINGW64 ~/Desktop/cpl (master)
   $ git status
   On branch master
   nothing to commit, working tree clean
   ```

   

4. 如果想要将忽略的文件也包含其中，可以使用`--all`或者是`-a`选项

5. 如果指定了`--patch`，Git就不会贮藏所有的修改，而是进入交互式界面让我们选择需要进行贮藏的部分修改进行贮藏

#### 从贮藏创建一个分支

1. 如果我们在贮藏了一些工作，然后继续在贮藏的分支上工作，那之后重新应用贮藏的时候可能就会出现问题。如果应用尝试修改刚刚修改的文件，你会得到一个合并冲突并不得不解决它。
2. 如果我们想要通过一个轻松的方式再次测试贮藏的改动，我们可以使用`git stash branch <branchname>`命令新建一个分支，并且会将贮藏的工作应用于这一个新的分支。我们就可以在应用成功后删除该贮藏的内容。
3. 这是一个可以轻松地在一个新的分支上恢复贮藏工作并且继续进行工作的一个不错的方法。

#### 清理工作目录

1. Git中可以使用`git clean`命令来删除工作目录中的一些目录和文件。
2. `git clean`命令会删除工作目录中未被追踪的文件。如果我们改变了主意，我们也不一定能够找回这些内容。
3. 一个更加安全的做法是`git stash --all`用来移除移除每一个目录和文件并且存放在栈中。
4. 我们可以使用`git clean`命令清理冗余文件和工作目录，使用`git clean -f -d`命令可以移除工作目录中所有的未被追踪的文件以及所有空的子目录。要使用`-f`选项要设置Git中的配置变量`clean.requireForce`没有显示为`false`。
5. 如果我们只是想看看`git clean`命令所做的事情，我们可以使用`--dry-run`或者是`-n`选项来做一次演示操作
6. 默认情况下，`git clean`只会移除没有被忽略的未跟踪文件。如果想要移除那些被忽略的文件，需要增加一个`-x`选项
7. 还可以使用`-i`选项进行交互式清理操作。
8. **==注意==**： 如果我们的工作目录中复制了别的仓库，这时候，我们即使使用`git clean -f -d`也不能够删除这些目录。这时候我们如果要删除这些目录，需要加上第二个`-f`选项。

### 搜索

不论仓库中的代码量有多少，我们经常需要查找一个函数是在哪里被调用和定义的，或者是我们想要了解一个方法的变更历史。Git提供了两个有用的工具用于快速地从其数据库中浏览代码和提交。

#### Git Grep

1. Git提供了一个`grep`命令，可以方便地从提交历史、工作目录甚至是索引中查找一个字符串或者正则表达式

2. 默认情况下`git grep`命令查找的是我们的工作目录下的文件。我们可以使用`-n`或者`--line-number`选项输出匹配行的行号。例如

   ```bash
   $ git grep -n main
   hello.c:3:int main(void){
   multipleAB.c:3:int main(void){
   ```

   

3. `-c`或者`--count`选项用于输出概述信息。这些信息包括那些包含匹配字符串的文件以及每一个文件包含了多少匹配。例如

   ```bash
   $ git grep -c int
   hello.c:2
   multipleAB.c:5
   ```

   

4. 如果我们还关心搜索字符串的**上下文**，那么可以使用`-p`或者是`--show-function`选项来显示每一个匹配的字符串所在的方法或者函数。例如

   ```bash
   $ git grep --show-function int
   hello.c:int main(void){
   hello.c:        printf("Hello C\n");
   multipleAB.c:int main(void){
   multipleAB.c:   int a;
   multipleAB.c:   int b;
   multipleAB.c:   printf("输入两个数\n");
   multipleAB.c:   printf("%d * %d = %d\n", a, b, a * b);
   ```

#### Git日志搜索

1. 有时候我们可能并不想知道某一项在哪里，我们更加关心的是这样的字符串是**什么时候引入的**

2. `git log`命令有许多强大的功能可以通过提交信息甚至是`diff`的内容来找到某个特定的提交

3. 使用`-S`选项来显示新增和删除某一个字符串的提交。例如

   ```bash
   $ git log -S a --oneline
   e8d6683 为multipleAB.c源程序添加了交互式输入的功能
   ce7e504 新建一个multipleAB的c源代码文件，用于计算两个整数的乘积
   7c3bc83 为hello.c添加了一段程序用于求两个整数的相加的结果
   67520cb 新建一个hello.c文件
   ```

   

4. 如果我们想要更加精确的搜索结果，我们可以使用`-G`选项来使用正则表达式搜索。

##### 行日志搜索

1. 行日志搜索是另一个相当高级并且有用的日志搜索功能。在`git log`后面跟上`-L`选项即可调用，它可以展示代码中一行或者是一个函数的历史

2. 例如，如果我们想要查看`main`函数在`multipleAB.c`中的每一次变更，可以使用如下的命令

   ```bash
   $ git log -L :main:multipleAB.c
   commit e8d66837996c4857c25ebd44a8a522918c765388
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 17:22:05 2020 +0800
   
       为multipleAB.c源程序添加了交互式输入的功能
   
   diff --git a/multipleAB.c b/multipleAB.c
   --- a/multipleAB.c
   +++ b/multipleAB.c
   @@ -3,6 +3,8 @@
    int main(void){
   -       int a=6;
   -       int b=10;
   +       int a;
   +       int b;
   +       printf("输入两个数\n");
   +       scanf("%d %d", &a, &b);
           printf("%d * %d = %d\n", a, b, a * b);
           return 0;
    }
   
   commit ce7e50424747351da5ea68108c673163e496c4a4
   Author: Square John <1042009398@qq.com>
   Date:   Sun May 31 15:12:37 2020 +0800
   
       新建一个multipleAB的c源代码文件，用于计算两个整数的乘积
   
   diff --git a/multipleAB.c b/multipleAB.c
   --- /dev/null
   +++ b/multipleAB.c
   @@ -0,0 +3,6 @@
   +int main(void){
   +       int a=6;
   +       int b=10;
   +       printf("%d * %d = %d\n", a, b, a * b);
   +       return 0;
   +}
   ```

### 重写历史

1. 很多时候我们可能想要修改我们的提交历史。这可能涉及到改变提交的顺序、改变提交中的信息或者是文件、将提交压缩或者是拆分，甚至是完全移除提交。
2. **我们应该注意，我们上面的这些操作都应该是在我们将我们进行的工作与他人进行共享之前在本地进行，一旦我们将仓库的内容进行了共享，就不应该随便重写这些提交的历史**

#### 修改最后一次提交

1. 修改我们的最近一次提交可能是所有修改历史提交操作中最常见的一个。

2. 对于我们的最近的一次提交，我们往往想做两件事：**一个是简单地修改提交信息，另外一个就是通过添加、移除或者是修改文件更改实际的提交内容**。

3. 如果只想要修改最近一次的提交信息，我们可以使用`git commit --amend`命令。执行这个命令之后，就会跳转到提交信息编辑界面供我们修改提交信息。

   - 最后一次提交信息修改前

     ```bash
     $ git log --oneline
     a32930d (HEAD -> master) 为hello.c新增加一行6
     db19a07 为各个文件新增一行4
     10c1009 为三个文件增加一行数字3
     7244031 每一个文件增加了一行数字2
     92e51d0 每一个文件中写入了一行数字1
     1577b44 清空所有文件
     d414efe 新建了一个newile文件，并在其中输入了一行文字
     e8d6683 为multipleAB.c源程序添加了交互式输入的功能
     df951bd 为readme新增加了一行
     345fecd 又给a.txt新增加一行
     de97888 新增一个a.txt文件
     ce7e504 新建一个multipleAB的c源代码文件，用于计算两个整数的乘积
     7c3bc83 为hello.c添加了一段程序用于求两个整数的相加的结果
     25dd4cc Merge branch 'newbranch'
     14e9ff1 删除了打印hello world这一行
     35241b9 为readme文件新增了一行
     2b3775c 修改了hello.c文件中的换行符位置错误
     62ed47d 新增一个readme文件
     a5d2b56 添加一行'Hello C'
     b1fc8a9 将换行符作为单独一行输出
     67520cb 新建一个hello.c文件
     ```

     

   - 最后一次提交信息修改之后

     ```bash
     $ git commit --amend
     hint: Waiting for your editor to close the file...
     [main 2020-06-13T03:07:11.693Z] update#setState idle
     [master 81fc22d] 为a.txt新增加一行6
      Date: Sat Jun 13 11:06:36 2020 +0800
      1 file changed, 2 insertions(+)
     
     helloworld@surface MINGW64 ~/Desktop/cpl (master)
     $ git log --oneline
     81fc22d (HEAD -> master) 为a.txt新增加一行6
     db19a07 为各个文件新增一行4
     10c1009 为三个文件增加一行数字3
     7244031 每一个文件增加了一行数字2
     92e51d0 每一个文件中写入了一行数字1
     1577b44 清空所有文件
     d414efe 新建了一个newile文件，并在其中输入了一行文字
     e8d6683 为multipleAB.c源程序添加了交互式输入的功能
     df951bd 为readme新增加了一行
     345fecd 又给a.txt新增加一行
     de97888 新增一个a.txt文件
     ce7e504 新建一个multipleAB的c源代码文件，用于计算两个整数的乘积
     7c3bc83 为hello.c添加了一段程序用于求两个整数的相加的结果
     25dd4cc Merge branch 'newbranch'
     14e9ff1 删除了打印hello world这一行
     35241b9 为readme文件新增了一行
     2b3775c 修改了hello.c文件中的换行符位置错误
     62ed47d 新增一个readme文件
     a5d2b56 添加一行'Hello C'
     b1fc8a9 将换行符作为单独一行输出
     67520cb 新建一个hello.c文件
     ```

     

4. 另一方面，如果我们想要修改最后一次提交的实际内容，其流程如下

   1. 先补上想做的修改，

   2. 然后暂存它们，

   3. 然后使用`git commit --amend`命令使用新的提交**代替**旧的提交

   4. 例如

      - 修改前的提交

        ```bash
        $ git log --oneline -5
        81fc22d (HEAD -> master) 为a.txt新增加一行6
        db19a07 为各个文件新增一行4
        10c1009 为三个文件增加一行数字3
        7244031 每一个文件增加了一行数字2
        92e51d0 每一个文件中写入了一行数字1
        ```

        

      - 修改后的提交

        ```bash
        $ git log --oneline -5
        8210372 (HEAD -> master) 为a.txt新增加一行6以及新增了一行7
        db19a07 为各个文件新增一行4
        10c1009 为三个文件增加一行数字3
        7244031 每一个文件增加了一行数字2
        92e51d0 每一个文件中写入了一行数字1
        ```

        

5. 使用`--amend`选项的时候要注意：这样的修改会改变提交的SHA-1校验和。它类似于一个小变基，所以我们在这之前已经将最后一次提交进行了推送，那么我们就不应该使用它。

6. 如果我们的修改是琐碎的（例如修改了一个比武或者是添加了一个忘记暂存的文件），那么这种时候我们可能不需要修改提交信息，那么我们就可以使用`--no-edit`选项跳过修改提交信息的步骤。具体命令如下

   ```bash
   $ git commit --amend --no-edit
   ```

### 重置揭秘

#### 工作流程

经典的Git工作流程是通过操纵下面这三个区域来以更加连续的状态记录项目快照的。

![image-20200613174548865](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200613182157005.png)

1. `git add`命令会将文件复制到`Index`中
2. `git commit`会取得`Index`中的内容并将其保存为一个永久的快照，然后创建一个指向该快照的**提交对象**，然后更新`master`来指向本次提交

#### 重置(reset)的作用

1. `git reset --soft HEAD~`会同步移动`HEAD`以及其所指向的指针
2. `git reset [--mixed] HEAD~`在移动`HEAD`以及其指向的指针之后会将`HEAD`所指向的分支的文件复制到`Index`中。值得一说的是，这个`--mixed`可以忽略，默认就是这个选项
3. `git reset --hard HEAD~`在`--mixed`的基础上，还会将`Index`中的内容复制到`Working Derictory`中

#### 通过路径来重置

1. `git reset [HEAD] <filename>`会将当前`HEAD`所指向的分支的提交中的指定文件恢复到`Index`中

2. 我们还可以指定一个提交版本（使用其校验和或者是部分校验和）中的文件用于进行恢复。例如

   ```bash
   $ git reset [HashCode] <filename>
   ```

#### 压缩

我们还可以使用`reset`命令进行压缩的提交。

1. 假设有一个提交，实际上是一个未完成的工作，属于接下来将要提交的一个中间过程，那么我们就可以使用`git reset --soft HEAD^1`将`HAD`以及其所指向的分支重置到最新提交的父提交上；
2. 然后我们再次运行`git commit`命令，就相当于我们丢弃了那个中间的提交，然后新创建了一个提交，这个提交指向那个被抛弃的中间提交的父提交。

#### 检出（checkout）

##### 不带路径

1. `git checkout <branchname>`和`git reset --hard <branchname>`类似。不过其有两点区别

   - 首先一点就是和`reset --hard`不同的是，`checkout`对于工作目录是安全的，它会通过检查以确保不会将已经更改的文件弄丢，而`reset --hard`直接不检查就覆盖工作目录中的内容

   - 另外一个是`checkout`只会移动`HEAD`，而不移动其所指的分支

   - 如图所示

     ![image-20200613182157005](https://cdn.jsdelivr.net/gh/Square-John/Image/img/image-20200613174548865.png)

##### 带路径

如果`checkout`指定一个文件路径，那么它就和`reset`一样不会移动`HEAD`，而是直接用`HEAD`所指向的提交中的相应文件更新索引，还会覆盖工作目录中的对应文件。这就有点像`git reset --hard [branch] <filename>`（当然，Git并不允许这样的操作）。因此，这对于工作目录也是不安全的。示例

```bash
$ git checkout -- <filename>
```



### END

