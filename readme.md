# Git

### 1.安装

 * 1.1下载地址：`https://git-scm.com/downloads`

 * 1.2 配置账号：

   ```
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   $ git config --list // 查看配置信息是否修改成功
   ```


### 2.创建仓库

#### 2.1:本地仓库-远程仓库:

* ```
  $ mkdir learngit  // 新建目录
  $ cd learngit  // 进入目录
  $ pwd  // 查看当前文件目录
  $ git init // 初始化
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
### 3.可视化工具

* **sourcetree:**`https://www.sourcetreeapp.com/`