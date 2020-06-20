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

1.  假设此时我们已经完成了开发，打算将`iss53`分支合并到`master`分支上，这时候和前面的合并操作没有什么分别

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

