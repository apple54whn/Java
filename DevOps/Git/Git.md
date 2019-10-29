> Git 和其它版本控制系统（包括 Subversion 和近似工具）的主要差别在于 Git 对待数据的方法。其它大部分系统以文件变更列表的方式存储信息。这类系统（CVS、Subversion、Perforce、Bazaar 等等）将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。
>
> Git 更像是把数据看作是对**小型文件系统的一组快照**。 每次你提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 **快照流**。
>
> ![Git 存储项目随时间改变的快照。](images/snapshots.png)
>
> 

# 安装

官网有下载安装教程：https://git-scm.com/downloads，也可以使用homebrew安装。

利用`git —-version`查看安装的Git版本。



# 配置

三个作用域，缺省为`global`

* `git config --local`，只对某个仓库有效，比 global 优先级高（配置文件在.git/config下）

  * HEAD文件里存储当前操作的分支名，如`ref: refs/heads/master`
  * config文件里存放就是配置信息
  * refs文件夹里存放heads（分支）和tags文件夹
  * objects文件夹里存放commit，blob，tree等

  > 可以通过`git cat-file -t ace1ed8923df..`来查看该Hash的对象类型；`-p`查看内容；
  >
  > 对象类型有commit，tree，blob。Git 是一个**内容**寻址文件系统。Git 的核心部分是一个简单的键值对数据库（key-value data store），你可以向该数据库插入任意类型的内容，它会返回一个键值，通过该键值可以在任意时刻再次检索（retrieve）该内容。
  >
  > ![简化版的 Git 数据模型。](images/data-model-1.png)
  >
  > ![image-20190721142046089](images/image-20190721142046089.png)

* `git config —-global`，对当前用户所有仓库有效（配置文件在~/.gitconfig下）

* `git config —-system`，对系统所有用户的仓库有效，一般不用

若要显示config的配置，只需加上`--list`即可

一般安装后需要配置name和email，配置完成后可以使用上述命令查看

```bash
git config --global user.name 'conanan'
git config --global user.email '54whn54@gmail.com'
```





# 常用命令

* 已有项目代码纳入 Git 管理。会在该文件夹下创建`.git`文件夹

  ```bash
  git init 项目所在文件夹
  #或
  cd 项目所在文件夹
  git init
  ```

  新建项目直接用 Git 管理。会在当前目录创建`your_project`文件夹，并在该文件夹中创建`.git`文件夹

  ```bash
  git init your_project
  ```

* 克隆仓库

  ```bash
  git clone https://github.com/libgit2/libgit2 [mylibgit] #这会在当前目录下创建一个名为 “libgit2”或自定义的目录
  ```

  

* 查看该**工作目录**中未被 Git 管理的和已被 Git 管理的文件

  ```bash
  git status
  ```

* 添加到 Git **暂存区**。后面可以跟**多个文件或文件夹，以空格隔开**；可以使用通配符`*`

  ```bash
  git add README.md
  git add -A #添加所有
  git add -u #添加所有在暂存区中改动的文件
  ```

* **提交暂存区中的文件**到 Git 仓库

  ```bash
  git commit -m 'first commit'
  ```

* 查看每次提交日志信息，有提交的Hash，作者（name + email），时间，提交的信息。

  ```bash
  git log
  ```

  

# 修改暂存区中文件名

一般会考虑修改工作目录文件名`mv`，删除暂存区中该文件`rm`，将新文件添加到暂存区。使用 Git 命令就可以很方便：

```bash
git mv readme readme.md
```



# 查看版本历史

* 查看当前分支每次提交日志信息，有提交的Hash，作者（name + email），时间，提交的信息。

  ```bash
  git log
  ```

* 查看最近n次

  ```bash
  git log -n2
  ```

* 只查看提交信息（前面还有个不知道什么的7位16进制码）

  ```bash
  git log --oneline
  ```

* 查看所有分支的log，若写分支名则是指定分支的log

  ```bash
  git log --all
  ```

* 图形化

  ```bash
  git log --graph
  ```

* 真图形化

  ```bash
  gitk
  ```

  







# 分离头指针

HEAD指向一个commit，但是没有绑定到一个分支上，在切换分支时有可能Git会删掉。根据提示创建个分支即可。