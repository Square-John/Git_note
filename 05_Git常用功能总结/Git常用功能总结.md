# Git常用功能总结

## Git基础

### 用户名和邮箱配置

```bash
$ git config [--global] user.name "<username>"
$ git config [--global] user.email "<useremail>"
```

- 如果省略`--global`选项，配置只对当前仓库生效
- 否则就对当前用户的所有仓库有效
- 就近原则，本地配置比全局配置的优先级更高

### 获取帮助

#### 获取详细帮助信息

```bash
$ git help <cmd>
$ git <cmd> --help
$ man git-<cmd>
```



#### 获取简略帮助信息

将上面的三个命令的`help`替换成`h`即可获得简略信息

### 获取仓库的两种方式

#### 初始化一个目录为一个新的仓库

1. 在某一目录执行以下命令，便可将该目录转变成一个Git仓库

	```bash
	$ git init
	```

	

2. 命令执行之后会生成一个`.git`文件夹

#### 克隆一个已经存在的仓库

1. 使用以下命令克隆一个仓库

	```bash
	$ git clone <url> [<name>]
	```

	

2. 其中`<url>`为需要克隆的仓库的地址，`<name>`是可选的，即克隆到本地的仓库的名字，如果没有这一选项，就在本地创建一个和克隆处名字一样的本地库

### 记录每次更新到Git仓库

#### 查看文件状态

``` bash
$ git status
```

#### 暂存新建或者是已修改的文件	

```bash
$ git add <filename>/<dirname>
```

- 其后也可以是一个目录
- 如果是目录，就递归地将目录中的已修改的文件暂存到**暂存区**

#### 状态简览

1. 使用`git status`命令的状态结果可能略显繁琐

2. 我们可以使用`--short`或者是`-s`选项，使得输出简化

	```bash
	$ git status -s
	```

#### 忽略文件`.gitignore`

可以在仓库根目录之下创建一个`.gitionore`文件，在其中指定不进行版本管理的文件

#### 文件差异对比

##### 工作区和暂存区的文件差异的对比

```bash
$ git diff [<filename>]
```

- 其中`<filename>`是可选的，如果没有，就会输出所有的文件的差异

##### 本地库和暂存区的文件差异对比

```bash
$ git diff --cached/--staged [<filename>]
```

##### 工作区和本地库的最新版的文件差异对比

```bash
$ git diff HEAD -- [<filename>]
```

#### 提交更新

1. 在编辑器中输入提交信息

	```bash
	$ git commit
	```

	- 该命令会将暂存区中的文件一次性全部提交到本地库

2. 使用`-m`选项携带提交信息

	```bash
	$ git commit -m "<msg>"
	```

3. 跳过使用暂存区域

	```bash
	$ git commit -a -m "<msg>"
	```

	- 该命令会将**==已跟踪==**的文件全部暂存然后一次性提交
	- 对于**==未跟踪==**文件，不会进行暂存和提交

#### 移除文件

1. 将三个区中的文件进行移除

	```bash
	$ git rm <filename>
	```

	

2. 保留工作区中的文件

	```bash
	$ git rm --cached <filename>
	```

	

3. 工作区和本地库版本不一致的文件，需要使用`-f`选项进行强制删除

4. 对于**未追踪**的文件，不能使用该命令，实际上直接使用`rm`命令删除就行了

#### 移动或者重命名文件

```bash
$ git mv <fromfile> <tofile>
```

这相当于

```bash
$ mv <from_file> <to_file>
$ git rm <from_file>
$ git add <to_file>
```

#### 查看提交历史

```bash
git log 
```

- 常用的选项

	|      选项       |                      说明                      |
	| :-------------: | :--------------------------------------------: |
	|       -p        |        按补丁格式显示每个提交引入的差异        |
	|       -n        |            仅输出最近的n条提交记录             |
	|     --stat      |          显示提交的文件修改的统计信息          |
	|   --shortstat   |      只显示--stat中最后的行数添加删除统计      |
	|   --name-only   |       仅在提交信息后显示已修改的文件清单       |
	|  --name-status  |         显示新增、修改和删除的文件清单         |
	| --abbrev-commit |         仅显示SHA-1校验和的前几个字符          |
	| --relative-date | 使用较短的相对时间而不是完整的日期格式显示日期 |
	|     --graph     |    在日志旁使用ASCII图形显示分支与合并历史     |
	|    --pretty     |            自定义日志的输出显示格式            |
	|    --oneline    |   --pretty=oneline --abbrev-commit合用的简写   |



- 限制日志输出长度的常用选项

	|        选项         |                             说明                             |
	| :-----------------: | :----------------------------------------------------------: |
	|      --author       |                 只显示指定作者修改的提交记录                 |
	|       --grep        |     只显示提交说明中包含--grep选项指定的关键字的提交记录     |
	|        -num         |                  只显示最后的num个提交记录                   |
	| --since or --after  |                 只显示指定日期之后的提交记录                 |
	| --until or --before |                 只显示指定日期之前的提交记录                 |
	|     --committer     |                  只显示指定提交者提交的记录                  |
	|         -S          | 只显示文件数据中添加或者是删除了包含-S选项指定的字符串的提交记录 |



### 撤销操作

#### 修补提交

1. 提交之后想要修改提交信息，不修改文件内容

	```bash
	$ git commit --amend
	```

	- 执行该命令之后Git会让你重新输入提交信息，然后覆盖上一次提交的信息

2. 不仅修改提交信息，还修改实际的提交内容

	- 先对文件进行修改

	- 再执行下面的命令

		```bash
		$ git commit --amend
		```

		

	- 最后就会使用新的提交覆盖掉上一次的提交

3. 仅修改提交内容，不修改提交信息

	```bash
	$ git commit --amend --no-edit
	```

#### 版本回退

##### 仅移动`HEAD`以及其所指的分支

```bash
$ git reset --soft HEAD~/HEAD^n
```

- 一个`~`号表示回退一个版本
- `^n`的`n`表示回退的版本数

##### 将`HEAD`所指向的分支指向的提交版本的文件复制到暂存区中

```bash
$ git reset [--mixed] HEAD~/HEAD^n
```



##### 覆盖工作区的内容

```bash
$ git reset --hard HEAD~/HEAD^n
```

##### 取消暂存的文件

```bash
$ git reset [HEAD] <file>
```

- 这实际上就是使用`HEAD`所指向的分支的当前提交的版本的`<file>`覆盖暂存区中的相应文件

##### 压缩

1. 在我们进行了一次提交之后，再次对文件进行修改

2. 然后执行如下命令

	```bash
	$ git reset --soft HEAD~ 
	```

	

3. 然后再执行

	```bash
	$ git commit
	```

	

4. 就相当于将上一次的提交给覆盖了。当然，实际上那个提交依然存在

#### 引用日志

使用如下命令可以查看引用日志

```bash
$ git reflog
```

#### 根据引用日志可以返回到更新的版本

1. 使用引用日志命令，找到想要到的指定版本的校验和

2. 执行如下命令

	```bash
	$ git reset --hard/--soft/[--mixed] <checksum>/HEAD@{<n>}
	```

3. 就可以回到前面的版本

#### 检出（checkout)

##### 不带路径（切换分支）

```bash
$ git checkout <point>
```

- 将`HEAD`指向`<point>`

##### 带路径（撤销对文件的修改）

```bash
$ git checkout -- <file>
```

- 实际上就是不移动`HEAD`，而是将`HEAD`所指的分支当前指向的提交版本的`<file>`覆盖暂存区中的对应文件，同时覆盖工作区中的对应文件

### 远程仓库的使用

#### 远程仓库查看

1. 查看远程库列表

	```bash
	$ git remote
	```

	

2. 查看远程库列表的详细信息

	```bash
	$ git remote -v
	```

	

#### 添加一个远程库

```bash
$ git remote add <name> <url>
```

- `<name>`是远程库的简写名称
- `<url>`是远程库的具体地址

#### 抓取远程库的数据到本地

```bash
$ git fetch <name>/<url>
```

- `<name>`远程库的简称
- `<url>`远程库的具体地址

#### 合并数据到本地分支

```bash
$ git merge <name>/<branch>
```

- 该命令将远程库`<name>`的`<branch>`分支的内容合并到当前的分支上

#### 拉取远程库上的指定分支的内容

``` bash
$ git pull <name>
```

- `<name>`为远程库的简称
- 要使用该命令，必须先将本地库的当前分支与远程库的分支进行关联
- 该命令会将远程库上的内容下载到本地并将和当前分支关联的远程分支与当前分支进行合并

#### 推送到远程库

```bash
$ git push <name> <master>
```

- `<name>`是远程库的名称
- `<master>`实际上可以是其他分支

#### 查看某个远程仓库

``` bash
$ git remote show <name>
```

- `<name>`为远程仓库名

#### 远程仓库重命名

```bash
$ git remote rename <oldname> <newname>
```

- 该命令也就是更改本地对于远程库的称呼，并非将远程仓库的仓库名字改了

#### 远程仓库的移除

```bash
$ git remote remove/rm <name>
```

- 该命令只是将和远程仓库关联的远程仓库简写名称从本地删除了，也就是说以后我们的仓库不能再连接相应的远程库了，而不是说我们真的把一个仓库从远程服务器上移除了

### 打标签

#### 列出标签列表

1. 列出所有标签

	```bash
	$ git tag
	```

	

2. 列出满足一定条件的标签

	```bash
	$ git tag -l/--list <regex>
	```

	- `<regex>`为一个筛选条件，可以是一个正则表达式

#### 创建标签

##### 创建附注标签

```bash
$ git tag -a <tagName> -m "<msg>"
```

- `<tagName>`为标签名
- `<msg>`为标签信息

##### 查看标签信息以及打标签的提交的提交信息

```bash
$ git show <tagname>
```

##### 创建轻量标签

```bash
$ git tag <tagname>
```

##### 附注标签和轻量标签的区别

附注标签附带这标签信息，轻量标签没有标签信息。轻量标签就相当于一个简单的标记。

#### 补打标签

这个和为当前版本的提交打标签类似，不过就是再最后增加一项，也就是想要打标签的提交的校验和（或者部分检验和）

#### 共享标签

1. 将某一个标签显示推送到远程服务器

	```bash
	$ git push <remote> <tagname>
	```

	- `<remote>`为远程库名称
	- `<tagname>`为要推送的标签名

2. 将所有标签一次性全部推送到远程服务器

	```bash
	$ git push <remote> --tags
	```

#### 删除标签

##### 删除本地标签

```bash
$ git tag -d <tagname>
```

##### 删除共享到远程服务器上面的标签

1. 第一种方式

	```bash
	$ git push <remote> : ref/tags/<tagname>
	```

	

2. 更直观的方式

	```bash
	$ git push <remote> --delete <tagname>
	```

#### 检出标签

```bash
$ git checkout <tagname>
```

- 这实际上就是将`HEAD`指向`<tagname>`所指向的提交
- 这回导致仓库处于头指针分离状态
- 在这种状态下，我们如果要进行新提交，一般要先新建一个分支

### Git别名

#### 为Git内部的命令起别名

```bash
$ git config [--global] alias.<title> '<cmd>'
```

- 其中，如果有`[--global]`，那么这个命令的别名就会在整个当前用户都有效，这些配置文件在Git的全局配置文件中
- 如果没有`[--global]`，那么这个别名就只在本仓库生效，配置信息在当前仓库的`.git`文件夹下的`.gitconfig`文件中
- `<title>`为命令别名
- `<cmd>`为要起别名的具体命令

#### 为Git外部的命令起别名

```bash
$ git config [--global] alias.<title> '!<cmd>'
```

- 可以看出，和为内部命令起别名的区别就是要在外部命令前添加一个`!`号



## Git 分支

### 查看当前仓库的所有分支

```bash
$ git branch
```



### 分支的创建

#### 创建分支

```bash
$ git branch <branchname>
```



####  切换分支

```bash
$ git checkout <branch>
```

或者是

```bash
$ git switch <branch>
```



#### 创建并切换分支

```bash
$ git checkout -b <branch>
```

或者是

```bash
$ git switch -c <branch>
```



#### 查看各分支所指向的提交对象

```bash
$ git log --decorate
```

#### 查看各分支的更加详细信息

```bash
$ git log --oneline --decorate --graph --all
```

#### 分支的合并

```bash
$ git merge <master> <dev>
```

- `<master>`表示要并入的主分支
- `<dev>`表示要被合并的分支
- 就是要将`<dev>`的内容合并到`<master>`
- 如果省略`<master>`分支，默认将`<dev>`分支合并到当前分支

#### 分支的删除

1. 删除已经进行了合并的分支

	```bash
	$ git branch -d <branch>
	```

	

2. 删除一个还没有和其他分支进行合并的分支

	```bash
	$ git branch -d -D <branch>
	```

	- `-D`表示强制删除

### 分支合并产生冲突的解决办法

1. `git status`查看冲突的文件
2. 对于有冲突的文件进行手动修正
3. `git add`将冲突文件暂存
4. `git commit`将暂存的文件进行提交
5. 合并完成

### 分支管理

#### 列出所有分支

```bash
$ git branch
```

#### 查看每一个分支的最后一次提交信息

```bash
$ git branch -v
```

#### 查看已经和指定分支进行了合并的分支

```bash
$ git branch --merged [<branch>]
```

- 如果省略后面的分支名，则默认为当前分支

#### 查看还没有和指定分支进行过合并的分支

```bash
$ git branch --no-merged [<branch>]
```



### 远程分支

#### 获取远程引用（包括分支、标签等）的完整列表

```bash
$ git ls-remote <remote>
```

或者是

```bash
$ git remote show <remote>
```

#### 将分支推送到远程库上

##### 远程库分支和本地分支名字相同

```bash
$ git push <remote> <branch>
```

##### 给远程分支起名字

```bash
$ git push <remote> <localName> : <remoteName>
```

#### 本地分支和远程分支的合并

##### 和已有分支合并

```bash
$ git merge [<localBranch>] <remote>/<remoteBranch>
```

- 将`<remote>/<remoteBranch>`分支合并到`<localBranch>`分支
- 若缺省本地分支名称，默认为当前分支

##### 基于远程分支新建一个本地分支

```bash
$ git checkout -b <newBranch> <remote>/<branch>
```

- 这相当于新建的分支的起点和远程分支的一致
- 这实际上就创建了一个**跟踪分支**`<newBranch>`

#### 跟踪分支

**跟踪分支**就是和远程分支关联起来了的本地分支。在一个跟踪分支上面执行`pull`命令，Git会抓取到正确的远程分支的内容，然后自动合并到跟踪分支上。

##### 创建跟踪分支

1. 常规创建方法

	```bash
	$ git checkout -b <newBranch> <remote>/<branch>
	```

	

2. 创建一个和远程分支同名的跟踪分支

	```bash
	$ git checkout --track <remote>/<branch>
	```

##### 设置跟踪分支

```bash
$ git branch -u/--set-upstream-to <remote>/<branch>
```

- 设置当前分支为跟踪分支，其上游分支为`<remote>/<branch>`

##### 上游快捷方式

可以使用`@{u}`或者是`@{upstream}`来引用跟踪分支的上游分支

##### 查看所有的跟踪分支

```bash
$ git branch -vv
```

#### 获取数据并合并

##### 抓取

```bash
$ git fetch <origin>
```

##### 合并

```bash
$ git merge [<localBranch>] <remote>/<branch>
```

##### 拉取

对于跟踪分支，可以直接使用

```bash
$ git pull [<remote>]
```

命令进行自动抓取和合并

#### 删除远程分支

```bash
$ git push <remote> --delete <branch>
```



### 变基

#### 普通的变基操作

```bash
$ git rebase <A> [<B>]
```

- 将`<B>`分支变基到`<A>`分支上

- 如果缺省`<B>`选项，就默认将当前分支变基到`<A>`分支上

- 变基之后还要进行快进合并操作才能完成最终的变基

	```bash
	$ git checkout <A>
	$ git merge <B>
	```

	

#### 高级变基操作

```bash
$ git rebase --onto <a> <b> <c>
```

- 将在`<c>`上但是不在`<b>`上的提交历史变基到`<a>`上

- 进行快进合并

	```bash
	$ git checkout <a>
	$ git merge <c>
	```

## Git工具

### 分支引用

我们可以使用一个分支指针来指定一个提交记录。此时该记录就是分支指针所指向的当前提交记录。例如

```bash
$ git show master
```

### 引用日志

1. 引用日志和一般日志的区别是：**引用日志记录了`HEAD`以及分支指针指向的变化**，而**日志就是记录了提交历史的变化**。引用日志会记录每一次`Head`或者是个分支指针的变化。**在我们刚克隆一个仓库的时候，所得仓库的引用日志是空的**，这说明引用日志记录的是本地仓库的指针在本地经历的变化。

2. 查看引用日志

	```bash
	$ git reflog
	```

	

3. 使用`show`查看具体的某一个引用日志

	```bash
	$ git show <checksum>/HEAD@{<N>}
	```

	

4. 查看一个指针在指定时间之前所在的位置

	```bash
	$ git show <branch>@{<time>}
	```

	

5. 使用如下命令可以查看类似于`git log`输出格式的**引用日志信息**

	```bash
	$ git log -g
	```

### 祖先引用

#### 脱节符（`^`）

1. 在一个指针之后添加一个脱节符`^`，表示该指针的父提交（它所指向的前一个提交）。

	```bash
	$ git show master^
	```

	- 表示显示`master`分支所指向的提交的父提交的信息

2. 在脱字符`^`之后再加上一个数字`n`，表示该指针的第`n`个父提交。如果一个引用没有这么多个父提交，就会出现引用错误

	```bash
	$ git show master^2
	```

	- 表示显示`master`分支所指向的提交的第二个父提交的信息。只有`master`分当前所指的提交对象是由合并得来的时候，该命令才能正确执行。

3. `windows`的`cmd`中，`^`号是一个特殊字符，所以需要使用双引号括起来

	```bash
	$ git show HEAD^ #ERROR
	$ git show "HEAD^" #OK
	$git show HEAD^^ # OK
	```

	

#### 波浪号（`~`）

1. 在一个指针之后加上一个`~`号，也表示该指针指向的提交的父提交

	```bash
	$ git show master~
	```

	

2. 在`~`号之后再增加一个数字`n`，表示该指针当前指向的提交沿着某一路径的回溯的第`n`个提交

	```bash
	$ git show master~2
	```

	- `master`的父提交的父提交

### 提交区间

#### 双点

```bash
$ git log <master>..<dev>
```

- 选出在`<dev>`分支上但是不在`<master>`分支上的提交记录

#### 多点

1. 在一个分支前面加上`^`或者`--not`表示不包含在该分支上的提交

2. 在分支名前加上`^`，表示只对其后的第一个分支的否定

	```bash
	$ git log ^<master> <dev>
	$ git log <dev> ^<master>
	```

	- 上面两条命令都表示在`<dev>`上但是不在`<master>`上的提交

3. 使用`--not`，表示对其后的所有分支的否定

	```bash
	$ git log --not <master> <dev>
	$ git log <master> --not <dev>
	```

	- 这两个命令效果不同
	- 第一个命令表示不在`<master>`和`<dev>`上的提交
	- 第二个命令表示在`<master>`上但是不在`<dev>`上的提交



#### 三点

```bash
$ git log <master>...<dev>
```

- 表示表示存在于`<master>`或者是`<dev>`分支上，但是不同时存在于这两个分支上的提交
- 也就是那些非`<master>`和`<dev>`同时共有的提交

在该命令之后添加一个`--left-right`选项能够显示每一个提交所属的分支，使得结果更加清晰

```bash
$ git log <master>...<dev> --left-right
```



### 贮藏

#### 普通贮藏

```bash
$ git stash [push]
```

- 该命令将工作区和暂存区中的所有没有提交的修改贮藏起来
- 贮藏之后工作目录和暂存区就会变为干净的状态

#### 贮藏应用

```bash
$ git stash apply
```

- 将最新的一次贮藏内容应用于当前分支

#### 查看贮藏列表

```bash
$ git stash list
```

#### 应用旧版本的贮藏

```bash
$ git stash apply stash@{<n>}
```

- `stash@{<n>}`可以通过查看贮藏列表获得

#### 应用贮藏并且将原本暂存的文件暂存

```bash
$ git stash apply --index
```

#### 删除贮藏内容

```bash
$ git stash drop stash@{<n>}
```

#### 应用贮藏之后删除该贮藏

```bash
$ git stash pop
```

#### 保留暂存区的内容

```bash
$ git stash --keep-index
```

#### 让贮藏包含新建文件

```bash
$ git stash --include-untracked/-u
```

#### 把所有文件（包括被忽略的文件）都贮藏

```bash
$ git stash -a/--all
```

#### 新建一个分支用于应用贮藏

```bash
$ git stash branch <branch>
```

### 搜索

#### Git grep

1. Git中提供了一个`grep`命令，用于在提交历史、工作目录甚至是索引中查找一个字符串

2. 该命令支持正则查找

3. 基本用法

	```bash
	$ git grep <regex>
	```

	- `<regex>`为匹配字符串

4. 输出匹配行行号

	```bash
	$ git grep --line-number/-n <regex>
	```

	

5. 只输出匹配结果的概略信息

	```bash
	$ git grep -c/--count <regex?
	```

	

6. 显示匹配结果的上下文(匹配结果所在的函数或者方法)

	```bash
	$ git grep -p/--show-function <regex>
	```

#### Git日志搜索

##### 搜索新增或者删除一个字符串的提交日志

```bash
$ git log -S <regex>
```

##### 搜索一行或者一个函数的提交历史

```bash
$ git log -L <regex>
```







## END







