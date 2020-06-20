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

