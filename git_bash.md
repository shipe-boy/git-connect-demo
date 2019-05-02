# 关于git命令行操作

	$ clear			清除命令行
	$ pwd			查看当前目录
	$ git init		初始化仓库
	$ git add 文件名		提交指定文件到暂存区
	$ git commit -m "描述信息"		将文件提交
	$ git status	查看状态

## 来一个例子看看

下面是我进行一系列操作的步骤和总结：
	

```
首先我从git文件夹里打开了命令行
	$ cd demo2
	$ git init	（初始化仓库）
	$ git status  (查看状态)
	$ git add demo2_bash.txt
	$ git status
	此时发现已经提交暂存区成功
```



	再次修改文件里内容后：(只将文件提交在暂存区)
	$ git status	(发现有提示)
	$ git add demo2_bash.txt
	$ git status	(发现提示文件已经添加到了暂存区)
	
	提交到暂存区后下班，第二天说昨天的修改不用了（也就是在暂存区退回）
	$ git reset HEAD demo2_bash.txt	(从暂存区的修改回归到工作区)
	$ git status	(发现有提示修改变动，此时工作区文件内容还是修改过后的)
	$ git checkout -- demo2_bash.txt	(工作区回归到修改之前了)
	$ git status	(OK)
	
	再次修改文件内容：（这次将文件提交了）
	$ git add demo2_bash.txt
	$ git commit -m "second commit"
	$ git status	(OK)
	
	此时发现这次修改没必要，要回到之前的：（已经commit之后退回）
	$ git log	（打印日志）如：
		commit 0a5df349d11b905353bc1aedd3b92afbe0b817d6 (HEAD -> master)
		Author: shipe <1370027889@qq.com>
		Date:   Sun Apr 28 21:43:41 2019 +0800
	
			second commit
	
		commit f4b958144840aa25e878a25c7901b42372c608cb
		Author: shipe <1370027889@qq.com>
		Date:   Sun Apr 28 21:31:22 2019 +0800
	
			bash commit demo2
	$ git reset --hard f4b958144840aa25e878a25c7901b42372c608cb	(git reset --hard commit号)
		此时文件回到了没修改之前的

把本地仓库清除干净：
	

```
    $ git rm demo2_bash.txt	(本地文件没有了)
	$ git status	(查看)
	$ git commit -m "delet demo2_bash"
	$ git status	(OK)
```

## 克隆远程仓库

首先看看文件里是否有git仓库

```
	$ pwd	查看当前目录
	$ ls -a		查看是否有.git的隐藏文件（没有才能克隆，执行一下操作）
	$ git clone 仓库地址
	$ git status	
	..........(在克隆的文件进行了一些操作后)
			（$ echo 'clone demo' >> clone.txt）	往clone.txt文件里追加一句话：clone demo
			 ($ ll  或者 $ ls)
			 ($ cat clone.txt)	查看文件里的内容
	$ git status	发现有文件没有被跟踪
	$ git add clone.txt
	$ git status	发现已经提交到了暂存区
	$ git commit -m "first clone"	提交到本地仓库
	$ git status	(OK,文件已经在本地仓库了)
	接下来将其提交到远程仓库github上
	$ git push		提交到远程仓库
	
		
```

