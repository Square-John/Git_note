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

