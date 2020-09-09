# Git

### 1.安装

 * 1.1下载地址：`https://git-scm.com/downloads`

 * 1.2 配置账号：

   ```
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   $ git config --list // 查看配置信息是否修改成功
   ```

* 如何退出vim 方法：一直按住esc，再连续按大写的Z两次就退出来了。

### 2.创建仓库

#### 2.1:本地仓库-远程仓库:

* ```
  $ mkdir learngit  // 新建目录
  $ cd learngit  // 进入目录
  $ pwd  // 查看当前文件目录
  $ git init // 初始化
  $ ls 查看当前分支所有文件
```
  
* 新建需要上传的文件

  * ```
    $ git add readme.txt  // 先添加到本地仓库， 可一次性添加多个文件
    $ git commit -m "wrote a readme file" // 提交 -m 后面为提交说明，必填,方便以后查询
    $ git status  //查看状态和版本对比
    $ git diff  //查看文件修改的详情
    $ git log  // 查看所有的提交记录
    $ git log --pretty=oneline // 简短显示提交记录
    $ tyep file  // 命令行查看某个文件的具体内容 linux 里面是cat file
    ```

* 版本回退

  * git reset ： 回滚到某个版本, 后面的版本都会消失（一般用于错误提交，不想保留后续的提交），如果回滚到某个版本修改文件提交过后再滚到最新的版本，之前所修改的文件是无效的

    ```
    $ git reset --hard HEAD^  // 回退到上个版本，HEAD~100 回退100个版本  ^在windows有bug,会出现more？解决办法：a:加“”,b:HEAD~1
    $ git reset --hard 版本号  // 回退到指定版本，版本号前几位就可以了
    $ git reflog // 查看所有的命令历史，方便版本切换
    ```

  * git revert： 如果想回退特定的某个版本修改，然后会生成一个新的版本，保留了特定版本后续的内容

    ```
    $ git revert -n 版本号  // 修改特定的版本
    $ git commit -m  // 重新提交修改  如果出现冲突就需要先执行 git add，提交完成指定到最新的版本
    ```

* 撤销修改
  * 1.未执行git add:  git restore file 可还原
  * 2.已执行git add :  先执行 git restore --staged file 还原到工作区然后再执行1的操作

* 删除文件

  * git rm file 或者手动删除文件  
  * 撤销删除：git restore   确定删除：git add/rm file
  * 如果已经执行commit的话需执行 git reset 

  #### ***推送到远程仓库：

  * 创建远程仓库

  * 命令行： ssh-keygen -t rsa -C "GitHub账号的注册邮箱"  生成秘钥

  * 复制C:\Users\Administrator\.ssh文件下的公钥到GitHub设置SSH and GPG keys中的SSH keys

  * ```
    git remote add origin git@github.com:your name/demo.git  
    git push -u origin master
    后续提交只需要 git push
    ```

#### 2.2:远程仓库-本地仓库:

	1.github创建仓库
	2.本地git下来：git clone https://github.com/your name/demo.git
### 3.分支管理

* 新建分支： git branch <name>
* 查看所有分支： git branch
* 切换分支: git switch/checkout  <name>
* 快捷创建并切换分支：git switch -c <name>  / git checkout -b <name>
* 合并某分支到当前分支：git merge <name>
* 删除分支： git branch -d <name>

 * 解决冲突：找到冲突文件手动解决冲突然后重新添加提交

 * 用`git log --graph`命令可以看到分支合并图。

 * 强制禁用`Fast forward`模式： git merge --no-ff -m "merge with no-ff" dev;

 * Bug分支：例如当需要修复master的bug时候，git stash 储存当前的工作进度，切换master创建bug分支issue-101解决问题再合并到master；

   * 恢复之前的工作区：换到之前的工作分支，git stash list 查看之前的工作进度

   * 1. git stash apply 恢复，git stash drop 删除stash内的内容
     2. git stash pop 恢复同时删除stash区内容
     3. 如果多次stash,git stash apply stash@{0}恢复指定的stash

   * 如果在master 修复过的bug，想要同步在子分支上修复：

      * ```
        git cherry-pick <commit>（修复bug时提交的commit编号）
        ```

* 强制删除：删除没有合并过的分支

  ```
   git branch -D <name>
  ```

* 多人协作

  * ```
     git remote /  git remote -v  查看运程库信息
    ```

    如果信息没有origin开头续执行：`git remote add origin git@github.com:xxse/xx.git`

  * 写作开发流程：

    * 1. 从远程库克隆拉取分支 git clone 

      2. 自己本地新建分支开发，最好是和远程建立同名的分支

         ```
         git checkout -b branch-name origin/branch-name
         ```

         如果报错fatal：‘XXX' is not a commit and a branch 'dev' cannot be created from it：

         则需执行：`git fetch origin`  `git remote update origin --prune`

         然后再去创建分支

      3. 推送到远程 `git push origin branch-name`，如果推送失败说明有冲突，就需要先执行 `git pull`，如果提示没有关联 `git branch --set-upstream branch-name origin/branch-name`然后在执行 git pull，如果修改了同一个文件还需手动解决冲突

### 4.标签

* 标签相当于给某个commit添加个别名，方便快速查找

 * 创建标签：

   ```
   git tag <tagname> 默认是 HEAD 
   也可以给指定的提交添加tag $ git tag v0.9 commit id 例如:$ git tag v0.9 f52c633
   ```

* 查看标签信息

  ```
  git tag 查看所有的标签
  
  git show <tagname> example： git show v1.0
  
  git tag -a v0.1 -m "version 0.1 released" 1094adb  给指定的提交添加标签并添加说明
  ```

* 操作标签

  ```
  git tag -d v0.1  //删除标签
  git push origin <tagname>  // 推送到远程
  git push origin --tags  // 一次性推送所有本地未推送标签
  删除已经推送到远程的标签：
  	1. 先本地删除 git tag -d <tagname>
  	2. git push origin :refs/tags/<tagname>
  ```

### 5.Gitee

 * 配置SSH

 * 新建仓库/直接关联Github导入

 * 同时关联github和gitee

   ```
   git remote -v //查看远程库配置信息
   git remote rm origin 删除已经关联的远程库
   关联github：
   	git remote add github ssh地址
   关联gitee:
   	git remote add gitee ssh地址
   ```

### 6.可视化工具

* **sourcetree:**`https://www.sourcetreeapp.com/`