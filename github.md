版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看：

```c
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```c
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

好了，现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```c
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

终于舒了口气，从输出可知，`append GPL`的commit id是`1094adb`，现在，你又可以乘坐时光机回到未来了。

### 小结

现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 添加远程库

在本地的learngit仓库下运行命令：

```
$gitremoteaddorigingit@github.com:Camellia-ks/Quadcopter-based-on-Air001.git
```

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

## SSH公钥

### Git报错解决：git@gitee.com: Permission denied (publickey).

### 完整报错信息

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/880iqcol41.webp)

### 报错原因

我查了很多资料，最后判定是在本地(或[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)上)没有生成SSH公钥

### 解决方法

#### 第一步

当你没有SSH公钥的时候，在Terminal中输入下面的命令：

```javascript
ssh-keygen -t rsa -C "1106425813@qq.com"
```



之后按回车键，会出现下面图示中的内容，不需要管出现的一些要输入的问题，一路回车即可，最终会生成SSH公钥。（如果重新生成的话会覆盖之前的SSH公钥）

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/htf0zkuwcf.webp)

#### 第二步

继续在Terminal中输入如下命令：

```javascript
ssh -v git@github.com
```

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/kj5h65v70w.webp)

#### 第三步

在Terminal中输入如下命令：

```javascript
ssh-agent -s
```

Terminal中会显示与下图中类似的信息

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/ytbvde7qzy.webp)

#### 第四步

在Terminal中输入如下命令：

```javascript
$ ssh-add ~/.ssh/id_rsa
```

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/ha6okxrasw.webp)

##### 注意：

可能有些朋友在操作上一步时，会出现问题，显示如下图中的信息

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/iomcd5bcc0.webp)

如果遇到这个问题，不要慌，在Terminal中输入如下命令：

```javascript
$ eval `ssh-agent -s`
```

紧接着再输入：

```javascript
$ ssh-add ~/.ssh/id_rsa
```

如图，问题已解决！完美！

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/o9nnw5l0zs.webp)

#### 第五步

根据第四步中生成的SSH公钥路径信息，找到id_rsa.pub，用文本方式打开，将里面的内容全部复制。

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/ccgehi94vo.webp)

复制完成后，进入你的Gitee(码云)，登录账号，按如下步骤进心操作：

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/9w2wyms0xo.webp)

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/dpn7yrme8c.webp)

![img](https://ask.qcloudimg.com/http-save/yehe-5637704/bs9vbz1tn4.webp)

如果你的邮箱收到信息，则公钥添加成功，这个问题自然也就解决了，接下来根据各自所需进行操作即可。