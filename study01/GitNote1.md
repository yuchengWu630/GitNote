# Git学习 - 介绍 
<br/>
首先要感谢廖雪峰大大提供的优质文章供我学习
<br/>
<br/>

>Git是目前世界上最先进的分布式版本控制系统.
> * 版本控制系统
>   Git的版本控制,可以做到记录每次文件的改动然后生成版本号.
>   具体到哪一个用户,具体增删改查了什么?时间是什么?
>   
> * 分布式
>  分布式版本控制系统,就是每一个操作者的电脑上都有一个完整的版本库,所以操作者都在本地进行操作,不仅提高效率,就算离开网络依然可以提交文件,查看历史版本,创建分支等等.
#### 参考图
![ckt](http://dbaplus.cn/uploadfile/2016/0621/20160621103540896.jpg?_=5857731)

### 对比集中式
>  * 集中式版本管理系统(SVN)
> 最大的特点就是拥有一个中央服务器.
> 操作者必须通过网络获取服务器中文件的最新版本,然后再进行操作,操作完后需要将文件在推送会服务器.
> 而且不同于Git,它拥有一个全局版本号.
> 储存方式也不同,Git是按元数据,SVN则是按文件.
> 分支管理也大相径庭

<br/>
<br/>
<br/>

# Git学习 - 安装Git
>因为我这里是windows操作系统,所以对于window操作系统如何安装Git做一个简单的记录.

* 首先我们要去下载windows版本的Git
[这个是windows版本Git的下载地址,因为是国外的源所以下载会比较缓慢](https://git-for-windows.github.io/)
* 下载完成后直接点击安装(一路下一步),安装完成后在开始菜单栏可以打开Git Bash运行一个控制台,就说明成功了.
* 安装完成后我们还需要配置一下,在命令行输入:
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
> 因为Git是分布式版本控制系统,每个电脑都必须有一个名字和一个邮箱"要求就是这样!"
> 上面代码中的 ``` --global ``` 参数,全局的意思,也就是说你这台电脑上的所有Git版本库都会使用这个配置,也就是使用这个用户名和email.
* 如果你想查看你是否添加成功,可以输入 ``` git config --list  ```

<br/>
<br/>
<br/>

# Git学习 - 创建版本库
> 版本库(repository),就是一个目录,该目录里的所有文件都将被Git管理,对所有文件进行的增删改查操作都将被跟踪记录,以便任何时候都可以'追踪'或'还原'.
1. 找一个合适的地方,创建一个空的目录:
```
$ mkdir myGitproject
$ cd myGitproject //cd 进入目录
$ pwd //查看当前所在目录
```
**Windows系统的话,请确保你的各种目录名都不包含中文**
2. 通过 `git init` 命令把这个目录变成Git可以管理的库:
```
$ git init
Initialized empty Git repository in G:/myGitproject/.git/
```
细心的我们发现目录下多了一个 ```.git``` 的目录,这个目录就是Git来跟踪管理版本库的,所以非常重要,**所以千万不能修改或者移动.**
如果你没有看到 ```.git``` 目录,那是因为默认隐藏了,通过 ```ls -ah``` 命令可以看见.
****
**将文件添加到库中**
>* 版本控制系统的作用原理导致,其只能个跟踪文本类型文件的改动.图片,视频这些文件,虽然也能管理,但是没办法跟踪文件内的变化,所以就算改动了,也只能记录文件大小而已,文件内容的改变无法跟踪.
>* windows的用户经历避免使用windows本家的记事本(因为可能会造成不必要的麻烦)

1. 要把文件添加进库我们就得先建一个文件.
```
$ mkdir demo1
$ cd demo1
$ vi readme.txt
//进入readme.txt文件中
//此时是命令模式,输入i进入编辑模式.
初学Git,好好加油
我是中国人,不会英文,哈哈
//输入文字后按ESC退回命令模式shift+:输入wq保存并退出
```
2. 用命令 ```git add``` 将我们需要的文件添加进库(实现跟踪).
```
$ git add readme.txt
```
这里我们会发现Git给我们返回了一堆信息
```
warning: LF will be replaced by CRLF in demo1/readme.txt.
The file will have its original line endings in your working directory.
```
这是因为windows中换行符为CRLF,而在Linux下换行符为LF,所以在执行add时提示警告.
不用在意
3. 用命令 ```git commit``` 告诉Git,把文件提交到库:
```
$ git commit -m "my a readme demo"
[master 8b4b63d] my a readme demo
//一个文件被改动成功,插入了3行内容
 1 file changed, 3 insertion(+)
```
```git commit``` 命令, ```-m``` 后面输入的是本次提交的说明.
### 为什么需要add,commit两步呢?
因为你可以add跟踪多个文件,然后用commit一次提交.
```
$ git add demo1.txt
$ git add demo2.txt demo3.txt
$ git commit -m "add 3 demos."
```
具体还涉及到Git存储的一些原理,后面会讲到.
****

<br/>
<br/>
<br/>

# Git学习 - 查看库状态和修改内容
OK,我们已经将readme.txt成功添加到Git库中,我们继续修改readme.txt的文件
```
初学git,好好加油
我是中国人,不会英文,哈哈
所以嘛,我爱国!
```
现在我们运行 ```git status``` 看看结果:
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

```
```git status``` 命令让我们时刻掌握库的状态,上面干啥我们,readme.txt被修改过,但还没提交修改.
所我们我们还想看看,是否能找到我对readme.txt修改的内容,这里使用 ``` git diff``` 这个命令,我们就可以查看修改的内容了.
```
YuCheng.Wu@DESKTOP-H8F3GTN MINGW64 /g/myGitproject/demo1 (master)
$ git diff 
warning: LF will be replaced by CRLF in demo1/readme.txt.
The file will have its original line endings in your working directory.
diff --git a/demo1/readme.txt b/demo1/readme.txt
index a504757..bf4e7e0 100644
--- a/demo1/readme.txt
+++ b/demo1/readme.txt
@@ -1,2 +1,3 @@
 初学git,好好加油
 我是中国人,不会英文,哈哈
+所以嘛,我爱国!
```
我们可以看到+加号表示我们添加了一句话,相反删除的话,就是-减号.
OK,现在我们知道了修改的内容,再把它提交到库就安心许多,提交修改和提交新文件一样是 ```git add``` ,所以我们需要再:
```
$ git add readme.txt
```
同样的提示换行符的警告,无视.
在执行第二步 ```git commit``` 之前,我们再运行 ```git status``` 看看库的状态.
```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme1.txt
```
这里 ```git status``` 告诉我们,将要被提交的修改有readme.txt,这样我们就可以放心的提交了:
```
$ git commit -m "my a readme demo"
[master 5sc343d] my a readme demo
 1 file changed, 1 insertion(+)
```
提交后我们再```git status```看看仓库的状态:
```
$ git status
On branch master
nothing to commit, working tree clean
```
Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。
****

<br/>
<br/>
<br/>

# Git学习 - 版本回退


> * 修改文件 -> ```git add``` 添加新跟踪 -> ```git commit``` 提交到Git库
> 
> * 我们发现这样不断的修改,提交修改,就好像玩RPG游戏,不断地保存记录,去通关,再保存记录,遇到打不过的BOSS就回档.
> 
> * GIt里叫 ```commit``` **保存快照**,一旦发生错误的操作,我们就可以通过最近的一个commit恢复,然后继续工作.

为了演示后面的效果,我们可以对文件在做几次修改提交.
现在我回顾了一下我的修改提交.

版本1 : wrote a readme file (我们通过```-m```参数提交的说明)

版本2 : my a readme demo

版本3 : append readme1

现在我们通过Git的 ```git log``` 命令显示提交日志
```
commit 04c6450581b1dea676d62a8a32a56ea86dfebed0
Author: yuchengWu <243370725@qq.com>
Date:   Sat Jul 8 21:13:31 2017 +0800

    append readme1

commit 8b4b63dc139180b591521849ae1debcdd94c45bf
Author: yuchengWu <243370725@qq.com>
Date:   Sat Jul 8 17:26:31 2017 +0800

    my a readme demo

commit 4cf6d8599ec0caed2c4458bff80774d83756aabb
Author: yuchengWu <243370725@qq.com>
Date:   Sat Jul 8 15:32:20 2017 +0800

    wrote a readme file
```
这里 ```commit``` 看到的一长串数字,这个是哈希SHA1,他是通过算法计算出的一个非常大的数字,保证一件事物的唯一性.
因为Git是分布的版本控制系统,我们后面作为团队工作的一份子,如果大家都用1,2,3....作为版本号,那就会发生冲突.

其他两个 ```Author``` 和 ```Date``` 我想就不用过多解释了.

如果你觉得输出信息太多,太繁杂,可以在 ```git log``` 后面加上 ``` --pretty=oneline ```参数:
```
04c6450581b1dea676d62a8a32a56ea86dfebed0 append readme1
8b4b63dc139180b591521849ae1debcdd94c45bf my a readme demo
4cf6d8599ec0caed2c4458bff80774d83756aabb wrote a readme file
```
#### Git中用 ```HEAD``` 表示当前版本

如果你想要上个版本就是 ```HEAD^``` ,上上个就是 ```HEAD^^``` ,当向上的数值太多时,我们可以用 ```HEAD~(你需要的number)```.

OK现在,我们要把当前版本号 append readme1 回退到 my a readme demo,这里就要用到 ```git reset``` 命令:
```
$ git reset --hard HEAD^
HEAD is now at 8b4b63d my a readme demo
```
这里显示 HEAD 现在已经退回到 my a readme demo 了,我们可以通过 ```cat``` 命令查看readme.txt的内容是否还原.
```
$ cat readme1.txt
初学git,好好加油
我是中国人,不会英文,哈哈
所以嘛,我爱国!
```
确实已经还原.但是我们发现这样的操作是有些危险的,我们直接把源文件也还原了,如果我不小心还原错了,会导致很多不必要的麻烦. 
>所以下面我来说说 ```git reset``` 的参数的区别
> * 第一个当然是刚刚我们使用的 ```--hard``` 它表示彻底回退到某个版本，本地的源码也会变为上一个版本的内容. 
> * 第二个是 ```--soft``` 它表示回退到某个版本。只回退了commit的信息.
> * 第三个是 ```--mixed``` 它也是默认方式,只保留源码，回退commit(保存)和index(索引)信息

但是都没有关系,因为我们可以通过提交的ID,就可以重新还原回去,如果你不记得提交ID,可以通过 ```git reflog``` 可以查看全部的变更记录:
```
4cf6d85 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
04c6450 HEAD@{3}: commit: append readme1
8b4b63d HEAD@{4}: commit: my a readme demo
4cf6d85 (HEAD -> master) HEAD@{5}: commit (initial): wrote a readme file
```
**不过在实际的工作中,会有太多的变更记录,所以还是小心的使用 ```git reset``` 避免增加多余的麻烦**

现在我们获得了提交的ID号,我们又想把内容还原,我们就可使用:
```
$ git reset --hard 04c6450
HEAD is now at 04c6450 append readme1
```
然后我们查看readme.txt的内容又回来了.

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的 ```HEAD``` 指针，当你回退版本的时候，Git仅仅是把 ```HEAD``` 从指向append readme1：
 
<br/> 
 
![](http://wx4.sinaimg.cn/mw1024/006RjiPOly1fhcwbiihbwj30cg0803yq.jpg)
![](http://wx2.sinaimg.cn/mw1024/006RjiPOly1fhcwbjo51wj30d307sdg2.jpg)
****
