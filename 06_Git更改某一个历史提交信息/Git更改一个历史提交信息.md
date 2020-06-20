## Git 更改一个历史提交信息的方法

### 问题

1. 对于有些时候，我们可能在进行了多次提交之后，查看日志之后发现在前面的某一个版本的提交信息写错了或者是乱码,我们想要重新修改这一个提交记录的提交信息.

2. 如图所示

	```mermaid
	graph LR
	b(b) --> a("a")
	c(c) --> b
	d(d) --> c
	m[master] --> d
	h[HEAD] --> m
	```

	

	- 这时候我们发现b提交的提交信息出现乱码
	- 我们要修正这个乱码应该怎样做呢？

### 解决方法

#### 方法1

1. 将`master`指针重置到`b`的位置

	```bash
	$ git reset --soft <b>
	```

	- 其中`<b>`为提交`b`的校验码

	- 执行该命令之后，就变成了

		```mermaid
		graph LR
		b(b) --> a("a")
		c(c) --> b
		d(d) --> c
		m[master] --> b
		h[HEAD] --> m
		
		```

		

2. 然后我们在这里新建一个分支

	```bash
	$ git branch <dev>
	```

	- 这之后就变成了

		```mermaid
		graph LR
		b(b) --> a("a")
		c(c) --> b
		d(d) --> c
		m[master] --> b
		h[HEAD] --> m
		dev[dev] --> b
		```

		

3. 然后我们还要将`master`指向最新的提交版本，也就是`d`

	```bash
	$ git reset --soft <d>
	```

	- `<d>`为提交`d`的校验和

	- 执行该命令之后，就变成了

		```mermaid
		graph LR
		b(b) --> a("a")
		c(c) --> b
		d(d) --> c
		m[master] --> d
		h[HEAD] --> m
		dev[dev] --> b
		```

		

		

4. 然后切换`HEAD`指向`<dev>`

	```bash
	$ git checkout <dev>
	```

	

5. 然后在`<dev>`分支上对最新版本提交做修补提交

	```bash
	$ git commit --amend
	```

	- 执行完毕之后,就相当于

		```mermaid
		graph LR
		b("b(b')") --> a("a")
		c(c) --> b
		d(d) --> c
		m[master] --> d
		dev[dev] --> b
		h[HEAD] --> dev
		```

		

6. 然后将`master`分支变基到`dev`上

	```bash
	$ git rebase master
	```

	- 变基完成之后就成为下图所示

		```mermaid
		graph LR
		b(b') --> a("a")
		c(c) --> b
		d(d) --> c
		m[master] --> d
		dev[dev] --> b
		h[HEAD] --> dev
		```

		

7. 然后快进合并

	```bash
	$ git merge master
	```

	- 快进合并之后就变成了如下所示

		```mermaid
		graph LR
		b(b') --> a("a")
		c(c) --> b
		d(d) --> c
		m[master] --> d
		dev[dev] --> d
		h[HEAD] --> dev
		```

		

8. 至此，对某个历史版本的提交信息的修改就完成了

### 方法2

1. `rebase`最好不要用到已经进行了推送的分支上，所以上面的方法1可以在提交还没有被推送到远程库上的时候可以用
2. 当然对于已经进行的推送，如果真的需要也可以使用，不过这肯定会拉仇恨，一定要提前通知你的合作者
3. **综上，我们在进行提交和推送的时候，最好是慎之又慎，最好不要出现上面这样的错误**。具体来说
	- 最好每一次提交之后就查看一下提交的日志，确保本次提交没有出现错误。即使出现了错误也能及时使用修补提交
	- 进行推送之前一定要再次查看日志进行确认避免发生上述的错误