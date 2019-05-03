# 关于git命令行操作

## 一、初步认识命令行的git命令

	$ clear			清除命令行
	$ pwd			查看当前目录
	$ git init		初始化仓库
	$ git add 文件名		提交指定文件到暂存区
	$ git commit -m "描述信息"		将文件提交
	$ git status	查看状态

### 来一个例子看看

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
	$ git reset --hard f4b958144840aa25e878a25c7901b42372c608cb	(git reset --hard commit号)（后面会介绍标签管理）
		此时文件回到了没修改之前的

把本地仓库清除干净：
	

```
    $ git rm demo2_bash.txt	(本地文件没有了)
	$ git status	(查看)
	$ git commit -m "delet demo2_bash"
	$ git status	(OK)
```

## 二、连接远程仓库

### 1、创建 SSH key

1. 在github上创建账号，点击头像进入Settings界面
2. 选择SSH and GPG keys，点击New SSH key来创建SSH key
3. title 随意写（自己看的懂），key就要在本地生成了。（保持页面，回到本地生成SSH key）
4. 在windows下查看`[c盘->用户->自己的用户名->.ssh]`下是否有`id_rsa`、`id_rsa.pub`文件，如果没有.ssh需要手动生成。 
5. 在本地的cmd窗口(何处打开的都行) `$ ssh-keygen -t rsa -C ``"youremail@example.com"` 
6. 进入`[c盘->用户->自己的用户名->.ssh]` 打开命令行 $ ll    查看文件，会发现有`id_rsa`、`id_rsa.pub`文件
7. $ cat id_rsa.pub      (会弹出一长串东西,就是SSH key,将其复制到github上)
8. SSH key  创建好之后，如何查看是否本地于github连通了呢
   1. $ ssh -T git@github.com
   2. 如果有让选择yes/no，选择yes
   3. 第一次连接可能会有Warning，再次输入同样命令就好了

### 2、添加（连接）远程仓库

在github上创建好新仓库时，下面有详细的操作流程，可以直接按照上面的步骤操作。下面是我自己的总结：

```
主要涉及的命令：
	$ git remote add origin 仓库地址
	$ git push origin master
	$ git push -u origin master
```

本地创建仓库，按照github上的操作流程进行操作：

```
$ ls -a		检查文件里是否已经有.git仓库了
$ echo "# webpack-demo" >> README.md	新建README.md文件
$ git init	初始化仓库
$ ls -a		此时会多出README.md文件和.git文件
$ git add README.md		添加到暂存区
$ git commit -m 'first commit'	添加到本地仓库
$ git remote add origin 远程仓库地址	将本地仓库与远程仓库关联
$ git push -u origin master		(-u 默认把本地的master和远程的master关联上，只有第一次时用写-u,再次push时和克隆后push时不需要)

```



## 三、克隆远程仓库

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

## 四、标签管理

廖雪峰：

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。 

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

涉及命令行操作：

```
$ git tag	查看所有标签
$ git tag 标签名		创建标签
$ git tag -a 标签名 -m "描述信息"		指定提交信息
$ git tag -d 标签名	删除标签
$ git push origin 标签名	标签发布
```

如何打标签：

```
$ git status	查看当前状态
    On branch master
    Your branch is up to date with 'origin/master'.

    nothing to commit, working tree clean

$ git tag	查看标签

$ git tag v1.0.1	新建标签

$ git tag
	v1.0.1

$ git push origin v1.0.1	推送标签
    Total 0 (delta 0), reused 0 (delta 0)
    To https://github.com/shipe-boy/git-connect-demo.git
     * [new tag]         v1.0.1 -> v1.0.1

```

标签推送到github上后如何查看？

​	在项目名旁边，点击 Branch:master 下拉框选择tag查看。

## 五、分支管理

主要涉及命令：

```
$ git branch 分支名	创建分支
$ git branch		查看分支
$ git checkout 分支名		切换分支
```

来操作下：

```

$ git branch
	* master

$ git branch feature	创建分支

$ git branch	查看分支，当前在master分支上
      feature
    * master

$ git checkout feature		切换到feature分支上
	Switched to branch 'feature'

$ git branch
    * feature
      master

$ git status
    On branch feature
    nothing to commit, working tree clean

$ echo "new branch" >> branch.txt		添加了文件和内容

$ git add branch.txt
    warning: LF will be replaced by CRLF in branch.txt.
    The file will have its original line endings in your working directory

$ git commit -m "new branch commit"
    [feature 6c6a796] new branch commit
     1 file changed, 1 insertion(+)
     create mode 100644 branch.txt

$ git status
    On branch feature
    nothing to commit, working tree clean

$ git checkout master		回到master分支上
    Switched to branch 'master'
    Your branch is up to date with 'origin/master'.

$ git branch
      feature
    * master

$ git merge feature			将feater分支合并到master分支上（前提是当前你要在master分支上）
    Updating ed9a19f..6c6a796
    Fast-forward
     branch.txt | 1 +
     1 file changed, 1 insertion(+)
     create mode 100644 branch.txt


$ git branch -d feature		删除分支
	Deleted branch feature (was 6c6a796).

$ git branch
	* master


```

