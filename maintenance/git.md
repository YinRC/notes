# 1. 安装和初始化配置

```bash
$ git -v # 查看 git 版本
git version 2.41.0.windows.3
```

## 1.1 git config

+ 缺省（Local）：本地配置，只对本地仓库有效
+ **--global**：全局配置，对所有仓库有效
+ --system：系统配置，对所有用户的所有仓库有效

```bash
$ git config --global user.name "1203" # 名字可以有空格
$ git config --global user.email 1203@gmail.com
$ git config --global --list
user.name=1203
user.email=1203@gmail.com
```

## 1.2 版本库 Repo

```bash
# 有两种建立仓库的方法
# 1. 初始化一个仓库
$ git init xxx
# 2. 从线上克隆一个已有的仓库
$ git clone xxx
```



# 2. 区域和文件状态

## 2.1 三个区域

**工作区** Working Directory `.git 所在的目录`

**暂存区 / 索引区** Staging Area / Index `.git/index`

**本地仓库** Local Repository `.git/objects`

<img src="E:\github\notes\maintenance\git.assets\image-20230818124007057.png" alt="image-20230818124007057" style="zoom:67%;" />

## 2.2 四个状态

**未跟踪** untrack

**未修改** unmodified

**已修改** modified

**已暂存** staged

<img src="E:\github\notes\maintenance\git.assets\image-20230818124251870.png" alt="image-20230818124251870" style="zoom:67%;" />

```bash
git status # 查看仓库状态
git add # 添加到暂存区
git commit # 提交暂存区中的内容
git log #查看仓库提交的历史记录，可以使用 --oneline 设置单行显示一条提交记录
```



# 3. git reset 回退版本

<img src="E:\github\notes\maintenance\git.assets\image-20230818125552589.png" alt="image-20230818125552589" style="zoom:67%;" />

```bash

$ cd repo
$ echo 111 > file1.txt
$ echo 222 > file2.txt
$ echo 333 > file3.txt

$ git add file1.txt
$ git commit -m "commit1"
[master (root-commit) 67ce0e6] commit1
 1 file changed, 1 insertion(+)
 create mode 100644 file1.txt

$ git add file2.txt
$ git commit -m "commit2"
[master 2948be9] commit2
 1 file changed, 1 insertion(+)
 create mode 100644 file2.txt

$ git add file3.txt
$ git commit -m "commit3"
[master 714af00] commit3
 1 file changed, 1 insertion(+)
 create mode 100644 file3.txt

$ git log --oneline
714af00 (HEAD -> master) commit3
2948be9 commit2
67ce0e6 commit1

$ cp -rf repo repo-soft
$ cp -rf repo repo-hard
$ cp -rf repo repo-mixed
```



## 3.1 soft 模式 reset only HEAD

```bash
# 用法
$ git reset -help | grep "\-\-soft"
usage: git reset [--mixed | --soft | --hard | --merge | --keep] [-q] [<commit>]
    --soft                reset only HEAD

714af00 (HEAD -> master) commit3
2948be9 commit2 # point to this
67ce0e6 commit1

# 需要指定 HEAD 指针指向的位置
$ git reset --soft 2948be9
$ git log --oneline
2948be9 (HEAD -> master) commit2
67ce0e6 commit1

$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file$..." to unstage)
        new file:   file3.txt

# 工作区中文件还在
$ ls
file1.txt  file2.txt  file3.txt

# 暂存区的文件也在
$ git ls-files
file1.txt
file2.txt
file3.txt

# 综上，soft 选项只是撤回了提交，而没有改变暂存区和工作区
```



## 3.2 hard 模式 reset HEAD, index and working tree

```bash
# 用法
$ git reset -help | grep "\-\-hard"
usage: git reset [--mixed | --soft | --hard | --merge | --keep] [-q] [<commit>]
    --hard                reset HEAD, index and working tree

714af00 (HEAD -> master) commit3
2948be9 commit2 
67ce0e6 commit1 # point to this

$ git reset --hard 67ce0e6
HEAD is now at 67ce0e6 commit1
$ git log --oneline
67ce0e6 (HEAD -> master) commit1

$ git status
On branch master
nothing to commit, working tree clean

$ ls
file1.txt

$ git ls-files
file1.txt
```



## 3.3 mixed 模式（默认）reset HEAD and index

```bash
$ git reset -help | grep "\-\-mixed"
usage: git reset [--mixed | --soft | --hard | --merge | --keep] [-q] [<commit>]
    --mixed               reset HEAD and index

714af00 (HEAD -> master) commit3
2948be9 commit2 
67ce0e6 commit1 # point to this

$ git reset 67ce0e6
$ git log --oneline
67ce0e6 (HEAD -> master) commit1

$ ls
file1.txt  file2.txt  file3.txt

$ git ls-files
file1.txt
```



# 4. git diff 查看差异

```bash
$ cat file3.txt
333

$ echo "hello" > file3.txt

# 比较工作区和暂存区
$ git diff file3.txt
warning: in the working copy of 'file3.txt', LF will be replaced by CRLF the next time Git touches it
diff --git a/file3.txt b/file3.txt
index 55bd0ac..ce01362 100644
--- a/file3.txt
+++ b/file3.txt
@@ -1 +1 @@
-333
+hello

# 文件放入暂存区，git diff 无显示
$ git add file3.txt
warning: in the working copy of 'file3.txt', LF will be replaced by CRLF the next time Git touches it
$ git diff

# 比较工作区和版本库
$ git diff head
diff --git a/file3.txt b/file3.txt
index 55bd0ac..ce01362 100644
--- a/file3.txt
+++ b/file3.txt
@@ -1 +1 @@
-333
+hello

# 比较暂存区和版本库
$ git diff --cached # git diff --staged 亦可
diff --git a/file3.txt b/file3.txt
index 55bd0ac..ce01362 100644
--- a/file3.txt
+++ b/file3.txt
@@ -1 +1 @@
-333
+hello
$ git commit -m "commit4"
[master 9567314] commit4
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git diff --cached

# 比较版本库之间的差异
$ git log --oneline
9567314 (HEAD -> master) commit4
714af00 commit3
2948be9 commit2
67ce0e6 commit1

$ git diff 2948be9 67ce0e6 # commit2 commit1
diff --git a/file2.txt b/file2.txt
deleted file mode 100644
index c200906..0000000
--- a/file2.txt
+++ /dev/null
@@ -1 +0,0 @@
-222

$ git diff 67ce0e6 2948be9 # commit1 commit2
diff --git a/file2.txt b/file2.txt
new file mode 100644
index 0000000..c200906
--- /dev/null
+++ b/file2.txt
@@ -0,0 +1 @@
+222

# 和上一次提交的版本比较
$ git diff head~ head
diff --git a/file3.txt b/file3.txt
index 55bd0ac..ce01362 100644
--- a/file3.txt
+++ b/file3.txt
@@ -1 +1 @@
-333
+hello
$ git diff head^ head
diff --git a/file3.txt b/file3.txt
index 55bd0ac..ce01362 100644
--- a/file3.txt
+++ b/file3.txt
@@ -1 +1 @@
-333
+hello

# 和之前的某个版本比较
$ git diff head~1 head
diff --git a/file3.txt b/file3.txt
index 55bd0ac..ce01362 100644
--- a/file3.txt
+++ b/file3.txt
@@ -1 +1 @@
-333
+hello
$ git diff head~2 head
diff --git a/file3.txt b/file3.txt
new file mode 100644
index 0000000..ce01362
--- /dev/null
+++ b/file3.txt
@@ -0,0 +1 @@
+hello
$ git diff head~3 head
diff --git a/file2.txt b/file2.txt
new file mode 100644
index 0000000..c200906
--- /dev/null
+++ b/file2.txt
@@ -0,0 +1 @@
+222
diff --git a/file3.txt b/file3.txt
new file mode 100644
index 0000000..ce01362
--- /dev/null
+++ b/file3.txt
@@ -0,0 +1 @@
+hello

# 只显示指定文件的差异内容
$ git diff head~3 head file3.txt
diff --git a/file3.txt b/file3.txt
new file mode 100644
index 0000000..ce01362
--- /dev/null
+++ b/file3.txt
@@ -0,0 +1 @@
+hello
```



# 5. git rm 删除文件

```bash
# 删除工作区中的文件
$ ls
file1.txt  file2.txt  file3.txt
$ rm file1.txt
$ ls
file2.txt  file3.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    file1.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git ls-files
file1.txt
file2.txt
file3.txt
$ git add file1.txt	# 更新暂存区的状态
$ git ls-files
file2.txt
file3.txt


```

