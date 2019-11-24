[TOC]



---

==part i==

# 1. git介绍

# 2. git的初始化

## 2.1 git配置

### （1）全局配置：设置你的名字和email

这些就是你在提交 `commit` 时的签名，每次提交记录里都会包含这些信息

`git config --global <配置名称>.<配置的值>`（注意：是用“.”链接的）

```shell
git config --global user.name "zhaoyiyi"
git config --global user.email "798998599@qq.com"
```



# 3. 获得一个Git仓库

两种获得方法：3.1和3.2

## 3.1 从已有的Git仓库中Clone

### （1）知道项目仓库地址（Git URL）

支持的协议：

1. ssh://
2. http(s)://
3. git://

### （2）`git clone`从远程仓库中克隆

```shell
cd /home/shiyanlou/
git clone https://github.com/shiyanlou/gitproject
```

### （3）查看克隆过来的这个仓库

```shell
cd /home/shiyanlou/
ls -ahl
cd gitproject
ls -a
```



## 3.2 新建仓库

【把未进行版本控制的文件进行版本控制】

### （1）新建一个项目目录

```shell
cd /home/shiyanlou
mkdir project
ls -hl
```

### （2）进入代码目录，`git init`创建并初始化Git仓库

```shell
$ cd /home/shiyanlou/project
$ git init
Initialized empty Git repository in /home/shiyanlou/project/.git/
$ git ls -a
.  ..  .git
```

# 4. Git仓库管理

## 4.1 工作区创建或修改文件

### （1）创建新文件

```shell
$ cd /home/shiyanlou/project
$ touch file1 file2 file3
$ echo "test" >> file1
$ echo "test" >> file2
$ echo "test" >> file3
$ ls -al
$ cat file1
$ cat file2
$ cat file3
```

### （2）对文件进行修改



## 4.2`git status`查看当前Git仓库状态

```shell
$ git status
On branch master

Initial commit

Untracked files:
   （use "git add <file>..."） to include in what will be committed）

       file1
       file2
       file3
nothing added to commit but untracked files present （use "git add" to track）
```

可以发现，有三个文件处于 `untracked` 状态，下一步我们就需要用 `git add` 命令将他们加入到缓存区（Index）

## 4.3 使用`git add`加入缓存区

### （1）先加到缓存区

```shell
$ git add file1 file2 file3
```
### （2）再次查看仓库状态

```shell
$ git status
On branch master

Initial commit

Changes to be committed:
    （use "git rm --cached <file>..." to unstage）

       new file: file1
       new file: file2
       new file: file3
```

### （3）查看修改-缓存区与工作区比较`git diff --cached`

进入到 `git diff --cached` 界面后需要输入 `q` 才可以退出

### （4）查看修改-工作区与工作区比较`git diff`

【如果没有`--cached`参数，`git diff` 会显示当前你所有已做的但没有加入到缓存区里的修改。】

## 4.4 使用`git commit`提交修改

### （1）从缓存区提交到版本库

当所有新建，修改的文件都被添加到了缓存区，我们就要使用 `git commit` 提交到本地仓库

```shell
$ git commit -m "add 3 files"
```

### （2）直接从工作区提交到版本库

`-a` 参数将所有没有加到缓存区的修改也一起提交。【注意： `-a` 命令不会添加新建的文件】

```shell
$ git commit -a -m "add 3 files"
```

### （3）`git status`继续查看状态

## 4.5 `git rm`删除操作

直接使用 `git rm` 命令删除后会自动将已删除文件的信息添加到缓存区，`git commit` 提交后就会将本地仓库中的对应文件删除

## 4.5 `git remote add`将本地仓库关联到远端服务器

```shell
git remote add origin https://github.com/zhao1-1/project.git
```

对于上述命令而言，`git remote add` 命令用于添加远程主机，`origin` 是主机名，此处我们可以自定义，不一定非要使用 `origin`

## 4.7 `git push`将本地仓库同步到远端服务器

```shell
$ git push origin master
```

【需要根据提示输入仓库对应的用户名和密码】

# 5. Git分支与合并

> Git 的分支可以让你在主线（master 分支）之外进行代码提交，同时又不会影响代码库主线。分支的作用体现在多人协作开发中，比如一个团队开发软件，你负责独立的一个功能需要一个月的时间来完成，你就可以创建一个分支，只把该功能的代码提交到这个分支，而其他同事仍然可以继续使用主线开发，你每天的提交不会对他们造成任何影响。当你完成功能后，测试通过再把你的功能分支合并到主线。

## 5.1 `git branch <branchname>`创建新分支“experimental”

```shell
$ git branch experimental
```

## 5.2 `git branch`查看当前分支列表

```shell
$ git branch
 experimental
* master
```

## 5.3 `git checkout <branchname>`切换分支

### （1）切换到新分支“experimental”

experimental 分支是你刚才创建的，master 分支是 Git 系统默认创建的主分支。星号标识了你当工作在哪个分支下，输入 `git checkout 分支名` 可以切换到其他分支。

```shell
$ git checkout experimental
Switched to branch 'experimental'
$ git branch
* experimental
 master
```



### （2）在分支“experimental”中修改文件file1并提交

```shell
# 修改文件file1
$ echo "update" >> file1
# 查看当前状态
$ git status
# 添加并提交file1的修改
$ git add file1
$ git commit -m "update file1"
# 查看file1的内容
$ cat file1
test
update
# 切换到master分支
$ git checkout master
```

查看下 `file1` 中的内容会发现刚才做的修改已经看不到了。因为刚才的修改时在 `experimental` 分支下，现在切换回了 `master` 分支，目录下的文件都是 `master` 分支上的文件了。

## 5.4 Git合并分支

### （1）分支修改无冲突：在master分支上修改文件file2

```shell
# 首先切换到master分支
$ git checkout master
# 修改文件file2
$ echo "update again" >> file2
# 查看当前状态
$ git status
# 添加并提交file2的修改
$ git add file2
$ git status
$ git commit -m "update file2 on master"
$ git status
# 查看file2的内容
$ cat file2
test
update again
```

### （2）`git merge` 命令来合并 experimental分支到主线分支 master

```shell
# 切换到master分支
$ git checkout master
# 将experimental分支合并到master
$ git merge -m 'merge experimental branch' experimental
```

`-m` 参数仍然是需要填写合并的注释信息

【**由于两个 branch 修改了两个不同的文件，所以合并时不会有冲突，执行上面的命令后合并就完成了。**】

### （3）分支有修改冲突：同时对两个分支的file3文件进行修改

1. 在master分支上修改file3 文件并提交

    ```shell
    # 切换到master分支
    $ git checkout master
    # 修改file3文件
    $ echo "master: update file3" >> file3
    # 查看状态
    $ git status
    # 提交到master分支
    $ git commit -a -m 'update file3 on master'
    ```

    

2. 切换到 experimental分支，修改 file3 并提交

    ```shell
    # 切换到master分支
    $ git checkout master
    # 修改file3文件
    $ echo "master: update file3" >> file3
    # 提交到master分支
    $ git commit -a -m 'update file3 on master'
    ```

    

3. 切换到 master 进行合并

    ```shell
    $ git checkout master
    $ git merge experimental
    Auto-merging file3
    CONFLICT （content）: Merge conflict in file3
    Automatic merge failed; fix conflicts and then commit the result.
    ```

    

4. 合并失败后先用 `git status` 查看状态，会发现 file3 显示为 `both modified`，查看 file3内容

    ```shell
    $ git status
    
    $ cat file3
    test
    <<<<<<< HEAD
    master: update file3
    =======
    experimental: update file3
    >>>>>>> experimental
    ```

    【上面的内容也可以使用 `git diff` 查看，先前已经提到 `git diff` 不加参数可以显示未提交到缓存区中的修改内容。】

    

5. 使用 vim 编辑这个文件，去掉 Git 自动产生标志冲突的 `<<<<<<` 等符号后，根据需要只保留我们需要的内容后保存

    ```shell
    $ vim file3
    ```

    

6. 使用 `git add file3` 和 `git commit` 命令来提交合并后的 file3 内容，这个过程是手动解决冲突的流程

    ```shell
    # 提交修改后的文件
    $ git add file3
    $ git commit -m 'merge file3'
    ```


## 5.5 `git branch -d <branchname>`删除分支

```shell
$ git branch -d experimental
```

【`git branch -d`只能删除那些已经被当前分支的合并的分支. 如果你要强制删除某个分支的话就用`git branch –D`】

## 5.6 `git reset --hard HEAD^`撤销一个合并

如果你觉得你合并后的状态是一团乱麻，想把当前的修改都放弃，你可以用下面的命令回到合并之前的状态：

```shell
$ git reset --hard HEAD^
# 查看file3的内容，已经恢复到合并前的master上的文件内容
$ cat file3
# 此时的file3已经回到了没合并之前的版本
test
master: updater file3
```

## 5.7 快速向前合并

>还有一种需要特殊对待的情况，
>
>一个合并会产生一个合并提交（commit）， 把两个父分支里的每一行内容都合并进来。
>
>但是，如果当前的分支和另一个分支没有内容上的差异，就是说当前分支的每一个提交（commit）都已经存在另一个分支里了，Git 就会执行一个 `快速向前`（fast forward）操作；Git 不创建任何新的提交（commit），只是将当前分支指向合并进来的分支。

```shell
$ cd /home/shiyanlou/project
$ git branch
$ ls
$ cat file1
$ cat file2
$ cat file3
$ git branch experimental2
$ git branch
$ git checkout experimental2
$ echo "add a new fix" >> file1
$ cat file1
$ cat file2
$ cat file3
$ git status
$ git add file1
$ git status
$ git commit -m "experimental2 branch add some fix in file1"
$ git status
$ ls
$ cat file1
$ cat file2
$ cat file3
$ git checkout master
$ git branch
$ ls
$ cat file1
$ cat file2
$ cat file3
$ git status
$ git merge -m "merge experimental2 branch" experimental2

Updating ff2b276..5f05b3a
Fast-forward (no commit created; -m option ignored)
file | 1+
1 file changed, 1 insertion(+)

$ pwd



```



# 6. Git日志

## 6.1 `git log`查看日志

```shell
$ git log
```

`git log` 命令可以显示所有的提交（commit）

如果提交的历史纪录很长，回车会逐步显示，输入 `q` 可以退出。

### `git log`选项

使用 `git help log` 查看。

> 例如：
>
> 下面的命令就是找出所有从 "v2.5“ 开始在 fs 目录下的所有 Makefile 的修改
>
> ```shell
> $ git log v2.5.. Makefile fs/
> ```
>
> Git 会根据 git log 命令的参数，按时间顺序显示相关的提交（commit）。



## 6.2 `git log --stat`日志统计

【显示在每个提交（commit）中哪些文件被修改了， 这些文件分别添加或删除了多少行内容，这个命令相当于打印详细的提交记录】

## 6.3 格式化日志

### （1）`git log --pretty=oneline`

### （2）`git log --pretty=short`

### （3）其他格式

>你也可用 `medium`，`full`，`fuller`，`email` 或 `raw`。 如果这些格式不完全符合你的需求， 你也可以用 `--pretty=format` 参数定义格式。

### （4）`git log --graph`

`--graph` 选项可以可视化你的提交图（commit graph），会用ASCII字符来画出一个很漂亮的提交历史（commit history）线

## 6.4 日志排序

### （1）默认情况，提交会按逆时间顺序显示

### （2）`git log --pretty=format:'%h : %s' --topo-order --graph`

指定 `--topo-order` 参数，让提交按拓扑顺序来显示（就是子提交在它们的父提交前显示）：

### （3）`git log --reverse`逆向显示

# 7. 撤销修改



---

==part ii==

# 8. 查看文件修改后的差异

## 8.1 `git diff`和`git diff --cached`比较提交

### （1）`git diff`比较当前工作区和当前缓冲区（当文件处于无法追踪状态即新添加的，则无法比较）

### （2）`git diff --cached`比较当前缓冲区和当前版本库

### （3）`git diff HEAD~n`比较当前工作区和历史版本库

### （4）`git diff <branchname>`比较当前工作区和某个分支当前版本

### （5）`git diff <版本1> <版本2>`比较v1和v2两个版本

### （4）实操：

1. 对项目做些修改：

    ```shell
    $ cd gitproject
    # 向README文件添加一行
    $ echo "new line" >> README.md
    # 添加新的文件file1,并向其写入一句“new file”
    $ echo "new file" >> file1
    $ ls -ahl
    $ cat README.md
    $ cat file1
    ```

    

2. 使用 `git status` 查看当前修改的状态：

    ```shell
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    
    Changes not staged for commit:
      （use "git add <file>..." to update what will be committed）
      （use "git checkout -- <file>..." to discard changes in working directory）
    
        modified:   README.md
    
    Untracked files:
      （use "git add <file>..." to include in what will be committed）
    
        file1
    
    no changes added to commit （use "git add" and/or "git commit -a"）
    ```

    【可以看到一个文件修改了，另外一个文件添加了】

3. 使用 `git diff` 命令比较修改的或提交的文件内容

    ```shell
    $ git diff
    diff --git a/README.md b/README.md
    index 21781dd..410e719 100644
    --- a/README.md
    +++ b/README.md
    @@ -1，2 +1，3 @@
     gitproject
     ==========
    +new line
    $ git diff --cached
    # 发现无输出
    ```

    【上面的命令执行后需要使用 `q` 退出】

    【命令输出当前工作目录中修改的内容，并不包含新加文件，请注意这些内容还没有添加到本地缓存区】

    【执行`diff --cached`命令发现两个文件都无比较，说明当前的版本库和缓冲区一致】

4. 先将新增的file1文件加入缓冲区并比较

    ```shell
    $ git add file1
    $ git status
    
    $ git diff
    diff --git a/README.md b/README.md
    index 21781dd..410e719 100644
    --- a/README.md
    +++ b/README.md
    @@ -1，2 +1，3 @@
     gitproject
     ==========
    +new line
    $ git diff --cached
    diff --git a/file1 b/file1
    new file mode 100644
    index 0000000..fa49b07
    --- /dev/null
    +++ b/file1
    @@ -0，0 +1 @@
    +new file
    ```

    【用`diff`命令时，发现还是没有file1文件的比较，因为工作区当前的file1文件和缓冲区当前的fie1文件并不区别】

    【用`diff`命令时，只有README.md文件的当前工作区和缓冲区的不同比较】

    【用`diff --cached`命令时没有README.md文件的比较，说明当前缓冲区和版本库并无区别】

    【用`diff --cached`命令时，file1文件缓冲区存在，版本库不存在，所以比较出差别】

5. 再将修改的README.md文件加入缓冲区并比较

    ```shell
    $ git add *
    $ git status
    
    $ git diff
    $ git diff --cached
    diff --git a/README.md b/README.md
    index 21781dd..410e719 100644
    --- a/README.md
    +++ b/README.md
    @@ -1，2 +1，3 @@
     gitproject
     ==========
    +new line
    diff --git a/file1 b/file1
    new file mode 100644
    index 0000000..fa49b07
    --- /dev/null
    +++ b/file1
    @@ -0，0 +1 @@
    +new file
    ```

    + 【执行`diff`的时候发现file1与README.md都没有变化，说明现在这两个文件的工作区和缓存区一致了】
    + 【执行`diff --chche`的时候，README.md文件缓存区与版本库不一致，file1文件缓存区有而版本库没有】

6. 先把README.md文件提交到版本库并比较

    ```shell
    $ git commit -m "update README.md" README.md
    $ git diff
    $ git diff --cached
    diff --git a/file1 b/file1
    new file mode 100644
    index 0000000..5b37f51
    --- /dev/null
    +++ b/file1
    @@ -0，0 +1 @@
    +new file
    ```

    + 【README.md文件提交到版本库后，`diff命令比较工作区和缓冲区发现无差别】

    + 【README.md文件提交到版本库后，`diff -cached`比较缓冲区和版本库发现也无差别】

    + 【file1文件没有提交版本库，`diff`命令比较工作区和缓冲区无差别】

    + 【file1文件没有提交版本库，`diff --cached`命令比较缓冲区和版本库发现版本库现在是空的，而缓冲区有文件】

7. 再把file1文件提交到版本库并比较：

    ```shell
    $ git commit -m "update file1" file1
    $ git diff
    $ git diff --cached
    ```

    【此时两个文件都提交后 `git diff` 与 `git diff --cached` 都不会有任何输出了】

## 8.2 比较分支

1. 首先创建一个新的分支 `test`，并在该分支上修改文件file1，新添加文件file2：

```shell
# 创建test分支并切换到该分支
$ git branch test
$ git checkout test
# 添加新的一行到file1
$ echo "branch test" >> file1
# 创建新的文件file2
$ echo "new file2" >> file2
# 提交所有修改
$ git add *
$ git commit -m 'update test branch'
```

2. 用`git diff`命令查看 test 分支和 master 之间的差别

    ```shell
    $ git diff master test
    diff --git a/file1 b/file1
    index fa49b07..17059cd 100644
    --- a/file1
    +++ b/file1
    @@ -1 +1，2 @@
     new file
    +branch test
    diff --git a/file2 b/file2
    new file mode 100644
    index 0000000..80e7991
    --- /dev/null
    +++ b/file2
    @@ -0，0 +1 @@
    +new file2
    ```

    

3. xxx

## 

## 8.3 更多的比较选项

### （1）`git help diff` 详细查看其他参数和功能

### （2）查看当前的工作目录与另外一个分支的差别

```shell
# 切换到master
$ git checkout master

# 查看与test分支的区别
$ git diff test
diff --git a/file1 b/file1
index 17059cd..fa49b07 100644
--- a/file1
+++ b/file1
@@ -1，2 +1 @@
 new file
-branch test
diff --git a/file2 b/file2
deleted file mode 100644
index 80e7991..0000000
--- a/file2
+++ /dev/null
@@ -1 +0，0 @@
-new file2
```



### （3）加上路径限定符，来只比较某一个文件或目录：

```shell
$ git diff test file1
diff --git a/file1 b/file1
index 17059cd..fa49b07 100644
--- a/file1
+++ b/file1
@@ -1，2 +1 @@
 new file
-branch test
```

【上面这条命令会显示你当前工作目录下的 file1 与 test 分支之间的差别。】

### （4）`--stat` 参数可以统计一下有哪些文件被改动，有多少行被改动：

```shell
$ git diff test --stat
 file1 | 1 -
 file2 | 1 -
 2 files changed， 2 deletions（-）
```



# 9. 分布式的工作流程

## 9.1 分布式工作流程

## 9.2 公共Git仓库

### （1）本地的某个目录名

```shell
$ git clone 仓库A的路径
$ git pull 仓库B的路径
```



### （2）ssh地址：比如GitHub

```shell
$ git clone ssh://服务器/账号/仓库名称
```



## 9.3 将修改推到一个公共仓库

> 通过 http 或是 git 协议，其它维护者可以通过远程访问的方式抓取（fetch）你最近的修改，但是他们没有写权限。如何将本地私有仓库的最近修改主动上传到公共仓库中呢？

用 `git push` 命令，推送本地的修改到远程 Git 仓库，执行下面的命令：

+ `git push ssh://服务器仓库地址 master:master`
+ `git push ssh://服务器仓库地址 master`

## 9.4 推送代码失败时该怎么办？

如果推送（push）结果不是快速向前 `fast forward`，可能会报像下面一样的错误：

```shell
error: remote 'refs/heads/master' is not an ancestor of
local  'refs/heads/master'.
Maybe you are not up-to-date and need to pull first?
error: failed to push to 'ssh://yourserver.com/~you/proj.git'
```

这种情况通常是因为没有使用 `git pull` 获取远端仓库的最新更新，在本地修改的同时，远端仓库已经变化了（其他协作者提交了代码），此时应该先使用 `git pull` 合并最新的修改后再执行 `git push`

```shell
$ git pull
$ git push ssh://服务器仓库地址 master
```



# 10. Git标签

## 10.1 轻量级标签

### （1） git tag 添加标签

用 git tag 不带任何参数创建一个标签（tag）指定某个提交（commit）:

```shell
# 进入到gitproject目录
$ cd /home/shiyanlou/gitproject

# 查看git提交记录
$ git log

# 选择其中一个记录标志位stable-1的标签，注意需要将后面的8c315325替换成仓库下的真实提交内，commit的名称很长，通常我们只需要写前面8位即可
$ git tag stable-1 8c315325

# 查看当前所有tag
$ git tag
stable-1
```

我们可以用stable-1 作为提交 `8c315325` 的代称。

前面这样创建的是一个“轻量级标签”。

### （2）标签对象

为一个tag添加注释，或是为它添加一个签名， 那么我们就需要创建一个 "标签对象"。

`git tag` 中使用 `-a`， `-s` 或是 `-u`三个参数中任意一个，都会创建一个标签对象，并且需要一个标签消息（tag message）来为 tag 添加注释。 如果没有 `-m` 或是 `-F` 这些参数，命令执行时会启动一个编辑器来让用户输入标签消息。

当这样的一条命令执行后，一个新的对象被添加到 Git 对象库中，并且标签引用就指向了一个标签对象，而不是指向一个提交，这就是与轻量级标签的区别。

```shell
$ git tag -a stable-2 8c315325 -m "stable 2"
$ git tag
stable-1
stable-2
```



## 10.2 签名的标签

签名标签可以让提交和标签更加完整可信。如果你配有`GPG key`，那么你就很容易创建签名的标签。

1. 设置GPGkey

    + 方法一：在你的 `.git/config` 或 `~/.gitconfig` 里配好key。

        ```shell
        $ cd /home/shiyanlou
        $ ls -a
        $ vim .gitconfig
        
        [user]
            signingkey = <gpg-key-id>
        ```

        

    + 方法二：命令行

        ```shell
        $ git config （--global） user.signingkey <gpg-key-id>
        ```

        

2. 在创建标签的时候使用 `-s` 参数来创建“签名的标签”：

    ```shell
    $ git tag -s stable-1 1b2e1d63ff
    ```

    

3. 如果没有在配置文件中配 GPG key，你可以用 `-u` 参数直接指定

    ```shell
    $ git tag -u <gpg-key-id> stable-1 1b2e1d63ff
    ```

    

---

==part iii==

> 从本节开始，我们会介绍一些中级和高级的用法，这些用法很少用到，前面实验的内容已经满足了日常工作需要，从本节开始的内容可以简单了解，需要的时候再详细查看。

# 11. 忽略某些文件

>项目中经常会生成一些 Git 系统不需要追踪（track）的文件。典型的是在编译生成过程中产生的文件或是编程器生成的临时备份文件。

> 当然，如果你不追踪（track）这些文件，平时可以不使用 `git add` 去把它们加到缓存区中，但是这样会很快变成一件烦人的事，你发现项目中到处有未追踪（untracked）的文件。这样也使 `git add .` 和 `git commit -a` 变得实际上没有用处，同时 `git status` 命令的输出也会有它们。

【你可以在你的顶层工作目录中添加一个叫 `.gitignore` 的文件，来告诉 Git 系统要忽略掉哪些文件。】

+ 以'`#`' 开始的行，被视为注释。
+ 忽略掉所有文件名是 foo.txt 的文件，`.gitignore` 的文件内容为：

    ```shell
    foo.txt
    ```

+ 忽略所有生成的 html 文件：

    ```shell
    *.html
    ```

+ 如果 `foo.html` 这个文件不能忽略，可以写个例外：

    ```shell
    !foo.html
    ```

+ 忽略所有 `.o` 和 `.a` 文件：

    ```shell
    *.[oa]
    ```

# 12. rebase



# 13. 交互式rebase

# 14. 交互式添加

# 15. 储藏

# 16. Git树名



---

==part iv==












