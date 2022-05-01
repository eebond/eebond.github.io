# Git版本控制教程（入门篇）

[本文参考：猴子都能懂的GIT](https://backlog.com/git-tutorial/cn/)

## 一、Git基础操作

### 1. 安装Git

本文按照命令行方式来安装和使用git，如需windows的GUI可以使用[tortoisegit](http://code.google.com/p/tortoisegit/)。

1. windows安装方式：<http://git-scm.com/>
2. liunx安装: `yum install -y git`

### 2. 初期设定

安装Git之后，请输入您的用户名和电子邮件地址。该设置操作在安装Git后进行一次就够了。这些信息将作为提交者信息显示在更新历史中。
> Git的设定被存放在用户本地目录的.gitconfig档案里。虽然可以直接编辑配置文件，但在这个教程里我们使用config命令。  

```bash
git config --global user.name "<用户名>"
git config --global user.email "<电子邮件>"
```

以下命令能让Git以彩色显示:
```bash
git config --global color.ui auto
```
可以为Git命令设定别名。例如：把「checkout」缩略为「co」，然后就使用「co」来执行命令。
```bash
git config --global alias.co checkout
```

 **Windows问题解决**  
 1. 报错`LF will be replaced by CRLF in xxxx.
The file will have its original line endings in your working directory.`  
原理：  
CRLF – Carriage-Return Line-Feed 回车换行    
就是回车(CR, ASCII 13, \r) 换行(LF, ASCII 10, \n)。  
这两个ACSII字符不会在屏幕有任何输出，但在Windows中广泛使用来标识一行的结束。  
而在Linux/UNIX系统中只有换行符。
也就是说在windows中的换行符为 CRLF， 而在linux下的换行符为：LF  
使用git来生成一个rails工程后，文件中的换行符为LF， 当执行git add .时，系统提示：LF 将被转换成 CRLF
```bash
git config core.autocrlf false  //禁用自动转换    
```
2. 如果如果在Windows使用命令行 (Git Bash), 含非ASCII字符的文件名会显示为 "\346\226\260\350\246..."。若设定如下，就可以让含非ASCII字符的文件名正确显示了。
 ```bash
 git config --global core.quotepath off
 ```
3. 若在Windows使用命令行，您只能输入ASCII字符。所以，如果您的提交信息包含非ASCII字符，请不要使用-m选项，而要用外部编辑器输入。
外部编辑器必须能与字符编码UTF-8和换行码LF兼容。
 ```bash
 git config --global core.editor "\"[使用编辑区的路径]\""
 ```

 ### 3. 新建本地数据库
 接下来要在本地新建数据库，创建一个名称为「tutorial」的空目录，并把它放在Git管理之下。  

 下面将以这个目录进行教程讲解。

首先在任意一个地方创建tutorial目录。然后使用init命令把该tutorial目录移动到本地Git数据库。  
```bash
git init
```
按照以下步骤把新创建的tutorial目录设置到Git数据库。
```shell
$ mkdir tutorial
$ cd tutorial
$ git init
Initialized empty Git repository in /Users/yourname/Desktop/tutorial/.git/
```

### 4. 提交文件
在tutorial目录新建一个文件，然后将文件添加到数据库。


首先在tutorial目录里新建一个名为「sample.txt」的文本文件，请在文件中输入以下的内容：
```txt
连猴子都懂的Git命令
```
请使用status命令确认工作树和索引的状态。
```bash
git status
```
执行status命令以确认tutorial目录的状态。
```bash
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#     sample.txt
nothing added to commit but untracked files present (use "git add" to track)
```
从status响应我们可以看到‘sample.txt’目前不是历史记录对象。请首先把‘sample.txt’加入到索引，就可以追踪它的变更了。  

将文件加入到索引，要使用add命令。在<file>指定加入索引的文件。用空格分割可以指定多个文件。  
```bash
$ git add <file>..
```
> tips
> 指定参数「.」，可以把所有的文件加入到索引。
> ```bash
> git add .
> ```
现在，我们把sample.txt加入到索引然后确认一下。
```bash
$ git add sample.txt
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#     new file:   sample.txt
#
```
既然sample.txt已加入到索引，我们就可以提交文件了。请执行如下显示的commit命令。  
```bash
$ git commit -m ""
```
执行commit命令之后确认状态。
```bash
$ git commit -m "first commit"
[master (root-commit) 116a286] first commit
 0 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 sample.txt

$ git status
# On branch master
nothing to commit (working directory clean)
```
从status响应我们可以看到没有新的变更要提交。

使用log命令，我们可以在数据库的提交记录看到新的提交。  
```bash
$ git log
commit ac56e474afbbe1eab9ebce5b3ab48ac4c73ad60e
Author: eguchi <eguchi@nulab.co.jp>
Date:   Thu Jul 12 18:00:21 2012 +0900

    first commit
```

## 二、共享数据库(远程仓库)

### 1. 创建共享数据库（远程仓库）
在GitHub或服务器中创建git数据库，比较简单此处不做演示

### 2. push到远程数据库

向远程数据库推送本地数据库的修改记录吧。
#### 给远程数据库取别名
您可以给远程数据库取一个别名。这样，下次推送的时候就不需要输入长串的远程数据库地址了。在这个教程里，我们的远程数据库命名为“origin”。

请使用remote指令添加远程数据库。在<name>处输入远程数据库名称，在<url>处指定远程数据库的URL。

```bash
git remote add <name> <url>
```
通过运行以下指令，将创建于上一个页面的远程数据库的URL命名为“origin”。
```shell
$ git remote add origin https://[your_space_id].backlogtool.com/git/[your_project_key]/tutorial.git
```
>执行推送或者拉取的时候，如果省略了远程数据库的名称，则默认使用名为”origin“的远程数据库。因此一般都会把远程数据库命名为origin。

#### push操作  
> GitHub仓库在push前，需要在GitHub中对本地数据库所在主机进行授权


使用push命令向数据库推送更改内容。< repository >处输入**目标数据库地址**或者**远程数据库别名**，< refspec >处指定**推送的分支**。我们将在高级篇详细地对分支进行说明。
```bash
git push <repository> <refspec>...
```
运行以下命令便可向远程数据库‘origin’进行推送。当执行命令时，如果您指定了-u选项，那么下一次推送时就可以省略分支名称了。但是，首次运行指令向空的远程数据库推送时，必须指定远程数据库名称和分支名称。
```shell
$ git push -u origin master
Username: <用户名>
Password: <密码>
Counting objects: 3, done.
Writing objects: 100% (3/3), 245 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://nulab.backlog.jp/git/BLG/tutorial.git
 * [new branch]      master -> master
```

### 3. 克隆远程数据库
快试试克隆远程数据库吧，这样您在别的地方也可以工作了。

使用clone指令可以复制数据库，在< repository >指定远程数据库的URL，
在< directory >指定新目录的名称。
```bash
git clone <repository> <directory>
```

执行以下指令后，会在目录(tutorial2) 复制远程数据库。
```bash
$ git clone https://nulab.backlog.jp/git/BLG/tutorial.git tutorial2
Cloning into 'tutorial2'...
Username: <用户名>
Password: <密码>
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
```
若要验证克隆是否成功，请看在复制的目录“tutorial2”中的sample.txt是否含有以下文字。
```txt
连猴子都懂的Git命令
```

### 4. 从克隆的数据库进行push
首先，在之前克隆的数据库目录里的sample.txt 添加以下黑体字，并提交。
```txt
连猴子都懂的Git命令
add 把变更录入到索引中
```
```bash
$ git add sample.txt
$ git commit -m "添加add的说明"
[master 1ef5c8c] 添加add的说明
 1 files changed, 1 insertions(+), 1 deletions(-)
 ```
 **用tutorial2进行的操作**  

 然后，推送此次变更，更新远程数据库。  
当在克隆的数据库目录执行推送时，您可以省略数据库和分支名称。
```bash
$ git push
Username: <用户名>
Password: <密码>
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 351 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://nulab.backlog.jp/git/BLG/tutorial.git
   486789c..1ef5c8c  master -> master
```
### 5. 从远程数据库pull
试试从远程数据库把最新变更内容拉取到tutorial吧。

我们把在上一页面中从“tutorial2”推送到远程数据库的内容拉取到数据库目录“tutorial”吧。  

使用pull指令进行拉取操作。省略数据库名称的话，会在名为origin的数据库进行pull。  

```bash
$ git pull <repository> <refspec>...
```

用tutorial进行的操作
```bash
$ git pull origin master
Username: <用户名>
Password: <密码>
From https://nulab.backlog.jp/git/BLG/tutorial
 * branch            master     -> FETCH_HEAD
Updating ac56e47..3da09c1
Fast-forward
 sample.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
 ```
 sample.txt文档的内容已更新。  

## 二、 整合修改记录
### 1. push冲突的状态
现在，我们将要学习怎样解决冲突。
首先，我们用“tutorial”和“tutorial2”制造一个冲突状态。

**用tutorial进行的操作**  
首先，打开tutorial目录的sample.txt文档，添加以下黑体字之后进行提交。  
```txt
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
```
```bash
$ git add sample.txt
$ git commit -m "添加commit的说明"
[master 95f15c9] 添加commit的说明
 1 files changed, 1 insertions(+), 0 deletions(-)
 ```
 **用tutorial2进行的操作**  
 接下来，打开tutorial2目录的sample.txt文档，添加以下黑体字之后进行提交。

```txt
连猴子都懂的Git命令
add 把变更录入到索引中
pull 取得远端数据库的内容
```
```bash
$ git add sample.txt
$ git commit -m "添加pull的说明"
[master 4c01823] 添加pull的说明
 1 files changed, 1 insertions(+), 0 deletions(-)
```

**用tutorial2进行的操作**  
现在从tutorial2 推送内容到远程数据库。  
```bash
$ git push
Username: <用户名>
Password: <密码>
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 391 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://nulab.backlog.jp/git/BLG/tutorial.git
   3da09c1..4c01823  master -> master
```

**用tutorial进行的操作**  
现在从tutorial推送内容到远程数据库吧。  
```bash
$ git push
Username: <用户名>
Password: <密码>
To https://nulab.backlog.jp/git/BLG/tutorial.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://nulab.backlog.jp/git/BLG/tutorial.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.
```

### 2. 解决冲突
为了把变更内容推送到远程数据库，我们必须手动解决冲突。首先请运行pull，以从远程数据库取得最新的变更记录吧。  

**用tutorial进行的操作**  
请执行以下指令。  
```bash
$ git pull origin master
Username: <用户名>
Password: <密码>
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://nulab.backlog.jp/git/BLG/tutorial
 * branch            master     -> FETCH_HEAD
Auto-merging sample.txt
CONFLICT (content): Merge conflict in sample.txt
Automatic merge failed; fix conflicts and then commit the result.
```

**用tutorial进行的操作**  
讯息显示「Merge conflict in sample.txt」。请打开sample.txt文件，我们看到Git已添加标示以显示冲突部分。请为Git无法完成主动合并的部分做以下的修改。  
```txt
连猴子都懂的Git命令
add 把变更录入到索引中
<<<<<<< HEAD
commit 记录索引的状态
=======
pull 取得远端数据库的内容
>>>>>>> 4c0182374230cd6eaa93b30049ef2386264fe12a
```
**用tutorial进行的操作**  
导入两方的修改，并删除多余的标示行以解决冲突。  
```txt
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
```

**用tutorial进行的操作**  
文件的内容发生了修改，所以需要进行提交。  
```bash
$ git add sample.txt
$ git commit -m "合并"
[master d845b81] 合并
```
这样就完成了从远程数据库导入最新的修改内容。

**用tutorial进行的操作**  
我们可以用log命令来确认数据库的历史记录是否准确。指定--graph选项，能以文本形式显示更新记录的流程图。指定--oneline选项，能在一行中显示提交的信息。  
```bash
$ git log --graph --oneline
*   d845b81 合并
|\
| * 4c01823 添加pull的说明
* | 95f15c9 添加commit的说明
|/
* 3da09c1 添加add的说明
* ac56e47 first commit
```
这表明两个修改记录已经整合了。

这时候，之前被拒绝的push应该可以通过了，push一下看看吧。


>辛苦了！Git的基本使用方法的说明到这里就告一段落了。有关分支以及修改等更高阶的内容，请参见高级篇！


