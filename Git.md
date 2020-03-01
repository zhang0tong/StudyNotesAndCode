<a id="top"></a>

# Git

<a href="https://www.bilibili.com/video/av52388193" target="_blank">视频教程</a>

- [Git](#git)
  - [一、Git](#%e4%b8%80git)
  - [二、下载、安装和配置](#%e4%ba%8c%e4%b8%8b%e8%bd%bd%e5%ae%89%e8%a3%85%e5%92%8c%e9%85%8d%e7%bd%ae)
  - [三、搭建git服务器（远程仓库）](#%e4%b8%89%e6%90%ad%e5%bb%bagit%e6%9c%8d%e5%8a%a1%e5%99%a8%e8%bf%9c%e7%a8%8b%e4%bb%93%e5%ba%93)
  - [四、常用命令](#%e5%9b%9b%e5%b8%b8%e7%94%a8%e5%91%bd%e4%bb%a4)
  - [五、三种状态以及分区](#%e4%ba%94%e4%b8%89%e7%a7%8d%e7%8a%b6%e6%80%81%e4%bb%a5%e5%8f%8a%e5%88%86%e5%8c%ba)
    - [1.三种分区](#1%e4%b8%89%e7%a7%8d%e5%88%86%e5%8c%ba)
    - [2.三种状态](#2%e4%b8%89%e7%a7%8d%e7%8a%b6%e6%80%81)
  - [六、分支操作](#%e5%85%ad%e5%88%86%e6%94%af%e6%93%8d%e4%bd%9c)
  - [七、版本穿梭](#%e4%b8%83%e7%89%88%e6%9c%ac%e7%a9%bf%e6%a2%ad)
  - [八、stash保存现场](#%e5%85%abstash%e4%bf%9d%e5%ad%98%e7%8e%b0%e5%9c%ba)
  - [九、其他总结](#%e4%b9%9d%e5%85%b6%e4%bb%96%e6%80%bb%e7%bb%93)
    - [1. tag标签](#1-tag%e6%a0%87%e7%ad%be)
    - [2. blame责任](#2-blame%e8%b4%a3%e4%bb%bb)
    - [3. diff差异性](#3-diff%e5%b7%ae%e5%bc%82%e6%80%a7)
    - [4. gc压缩](#4-gc%e5%8e%8b%e7%bc%a9)
    - [5. 裸库](#5-%e8%a3%b8%e5%ba%93)
    - [6. submodule（子模块）](#6-submodule%e5%ad%90%e6%a8%a1%e5%9d%97)
  - [十、团队协作开发](#%e5%8d%81%e5%9b%a2%e9%98%9f%e5%8d%8f%e4%bd%9c%e5%bc%80%e5%8f%91)
  - [十一、Git GUI](#%e5%8d%81%e4%b8%80git-gui)
    - [1. gui](#1-gui)
    - [2. gitk](#2-gitk)
    - [3. github desktop](#3-github-desktop)

## 一、Git

1. 分布式版本管理工具
2. 本地仓库又称为分支，默认master
3. 优势
   1. 本地版本控制
   2. 重写提交说明
   3. 回滚（rollback）
   4. 分支系统（branck）
   5. 全量（每一个版本都包含全部的文件，可以时刻保持数据完整性）

## 二、下载、安装和配置

1. 在[git官网](https://git-scm.com/)下载，安装。

2. 一般只需要选Use Git from Git Bash only，windows下其余的默认下一步。

3. 配置path

4. 配置git：用户名和邮箱

   ```bash
   git config --global user.name 'username'
   git config --global user.email 'xxx@xxx.com'
   ```

## 三、搭建git服务器（远程仓库）

1. 使用统一的托管网站<a href="https://github.com/" target="_blank">GitHub</a>

2. 为了在本地和远程仓库之间进行免密登陆，配置ssh

   1. 在本地生成ssh：`ssh-keygen -t rsa -C email`后面的不懂一直回车就行
   2. 发送到远程：在github个人页面settings -> SSH and GPK keys -> new SSH，title起个标识，key中粘贴刚生成的id_rsa.pub中的内容，去掉复制时的最后那个换行符（如果有则要勾选可写权限）。
   3. 这种方式创建的是当前账户的密钥，也可以为当前仓库进行配置

3. 测试连通性：`ssh -T git@github.com`输入yes，出现successfully即为成功

4. 在本地新建仓库，并发送到远程仓库

   1. 本地创建文件夹，然后进入，运行`git init`

   2. 在远程仓库创建空仓库

   3. 关联本地和远程仓库：git remote add origin git@github.com:/用户名/仓库名.git(git@github.com可以用https://github.com代替，或者直接复制项目的ssh地址)

   4. 解除关联：git remote remove origin

   5. 第一次发布项目（本地-远程）
   
      ```bash
      git add . #文件存到暂存区
      git commit -m "注释信息" #暂存区到本地分支（默认master）
   git push -u origin master #将本地分支提交到远程分支
      ```

   6. 第一次下载项目（远程-本地）：git clone git@github.com/用户名/仓库名.git（同上）

   7. 提交（本地-远程）
   
      ```bash
      git add .
      git commit -m "注释信息"
   git push #后续的提交无需标示符
      ```
   
   8. 更新（远程-本地）`git pull`（pull = fetch + merge）

5. 远程分支操作
   1. `git push -u origin master`会在本地建立一个用来感知远程状态的分支（称为：追踪分支/本地的远程分支），默认为`remotes/origin/master`，push的时候会使该指针指向最新一次的提交，也就是远程仓库的最新一次提交，其他用户可以通过`git remote show origin`查看远程的更新情况
   2. push到远程往往需要先pull，解决现有冲突，然后重新add，commit，最后push
   3. 查看github分支的日志：git log refs/remotes/origin/master
   4. 将本地的新分支推送到远程，需要关联推送`git push -u origin branch_name`；`git push --set-upstream origin test`，并且会在远程建立推送的新分支（`git push origin src:test`表示用本地的src分支在远程建立test分支）
   5. 远程建立或推送新分支后，其他用户会有新的追踪分支，然后创建新的同名本地分支并与追踪分支关联`git checkout -b new_branch_name origin/new_branch_name`（远程新的追踪分支的名字），或者`git checkout -b new_branch_name --track origin/new_branch_name`（`git checkout --track origin/new_branch_name`会将追踪分支的名字作为新的本地分支的名字）
   6. 删除远程分支`git push origin  :test`，表示用空替换test分支，即为删除；`git push origin --delete test`（只删除远程，不会删除本地的）
   7. 检测无效的追踪分支`git remote prune origin --dry-run`，清理无效的追踪分支`git remote prune origin`
6. 远程标签操作
   1. 推送标签到远程仓库`git push origin tag_name`（可以同时推送多个，一次用空格隔开），git push origin —tags（一次推送所有的标签）
   2. 获取远程标签`git pull`（如果远程新增标签，则pull可以将新增的标签拉去到本地；如果远程是删除标签，则pull无法感知）；`git fetch origin tag tag_name`
   3. 删除远程标签`git push origin  :refs/tags/tag_name`（与空替换远程标签，如果将远程标签删除，其他用户无法直接感知，还需要通过pull感知）

## 四、常用命令

1. git add：将本地文件从工作区提交到暂存区
2. git commit：将暂存区的内容提交到对象区
3. git push：将对象区内容推送到远程仓库
4. git pull：将远程仓库的东西拉取到本地仓库
5. git init：为当前目录添加版本管理
6. git status：查看当前状态
7. git rm --cached \<filename\>：从暂存区到工作区
8. git reset head \<filename\>：从暂存区到工作区
9. git checkout -- \<filename\>：从工作区到对象区
10. git rm \<filename\>：删除已提交的文件，会被放到暂存区，即从对象区到暂存区
11. git commit -m "注释信息"：彻底删除
12. git log：查看提交日志（git log --pretty=format:"%h - %an, %ar: %s" sha1校验码，作者，时间，注释；git log --graph, git log --graph --pertty=oneline --abbrev-commit）
13. git commit --amend -m "重写的注释信息"：重写注视（不能用于第一次）
14. git commit -am "注释信息"：add commit 同时进行
15. git reflog：查看全部记录
16. git branch -m master master2：分支重命名
17. git remote show：查看当前的服务器列表 

## 五、三种状态以及分区

### 1.三种分区

1. 工作区（正常使用的项目区域，add后为暂存）
2. 暂存区（add后的暂时存储，commit后为对象）
3. 对象区（通过对象区与其他仓库进行交互）
4. 对象区和暂存区统称为版本

### 2.三种状态

1. 已修改（modified）
2. 已暂存（staged）
3. 已提交（commited）

## 六、分支操作

1. 分支：一个commit链，一条记录工作线
2. 分支名（master）：指向当前的提交（commit）
3. 常用的分支
   1. dev：开发分支，频繁修改
   2. test：基本开发完毕后，交给测试实施人员的分支
   3. master：生产阶段，很少改变
   4. bugfix：临时修复bug分支
4. HEAD：指向当前分支名，用来标示该分支（HEAD -> 分支名）
5. 快照：又叫version，一次提交后的所有文件。
6. fast forward：如果一个分支靠前(dev)，另一个落后(master)。如果不冲突，master可以通过merge追赶上dev，称为fast forward。本质就是分支指针的移动，跳过的中间commit，仍然会保存。两个分支fast forward后归于一点，没有分支信息（丢失分支信息）；如果冲突，解决冲突（会先将另一个分支的提交信息拿过来，删除冲突中的提示，修改成最终需要的内容），再次提交（`git add .` ` git commit -m "注释信息"`这次的add为提交冲突已解决），此过程进行了两次提交。
7. 常见操作
   1. 查看分支：git branch
   2. 创建分支：git branch branch_name
   3. 切换分支：git checkout branch_name
   4. 删除分支：git branch -d branch_name(不能删除当前分支和包含未合并内容的分支，删除前先合并)
   5. 强制删除：git branch -D branch_name
   6. 创建新分支并切换：git checkout -b branch_name
   7. 合并分支：git merge 要合并的分支名（默认fast forward 不需要的话 --no-ff
8. 在新分支中创建的文件，如果不进行写操作，在其他分支中可见，否则不可见；在新分支的工作区进行写操作但没有add等git操作，可以直接删除新分支。或者说工作区各个分支是共享的。

## 七、版本穿梭

1. 回退到上一次commit：git reset --hard head^（有几个^回退几次）
2. 回退到上n次commit：git reset --hard head~n
3. 回退到指定次commit：git reset -am sha1值的前几位
4. 放弃工作区中的修改（相对于暂存区或对象去）：git checkout
5. 将上一次增加到缓存区中的内容回退到工作区：git reset
6. 版本穿梭（游离状态）：git checkout sha1值（修改后、必须提交，创建分支的好时机）

## 八、stash保存现场

1. 建议（规范）：在功能未开发完毕时，不要commit
2. 规定（必须）：在没有commit之前，不能切换分支
3. 保存现场：git stash
4. 查看已将保存的现场：git stash list
5. 还原现场：git stash pop（将原来保存的现场删除，用于还原内容，默认还原最近的现场）
6. 还原现场（不删除原现场）：git stash apply [索引]（需要手动删除，git stash 索引或者sha1值 ）
7. 如果还没有将某一个功能开发完需要切换，先保存现场

## 九、其他总结

### 1. tag标签
   1. 一般当项目比较完整可以发布时使用标签，相当于项目的发行版Release
   2. 标签适用于整个项目，和具体的分支没关系，简单标签存储当前commit的sha1值（git tag tag_name），带有注释的标签包含当前commit的sha1值（git tag -a tag_name -m "注释信息"）
   3. 创建标签：git tag tag_name/git tag -a tag_name -m "注释信息"
   4. 查看标签：git tag
   5. 删除标签：git tag -d tag_name
### 2. blame责任
   1. git blame file_name：查看该文件所有commit的sha1值，以及每一行的作者
### 3. diff差异性
   1. 比较文件本身，git diff比较区中的文件
   2. diff file_a file_b：比较两个文件的不同并显示
   3. diff -u file_a file_b：已a文件为基础，显示如果a文件加上或者删除某些行会和b文件相同
   4. git diff：比较暂存区和工作区中的差异
   5. git diff commit的sha1值：指定对象区与工作区比较
   6. git diff --cached commit的sha1值：对象区与暂存区的比较
### 4. gc压缩
   1. git gc：对文件进行压缩
### 5. 裸库
   1. 没有工作区的仓库（一般只在远程仓库）
   2. 通过本地建立一个裸库，然后推送到远程仓库`git init --bare`
### 6. submodule（子模块）
   1. 应用场景：在一个仓库中，引用另一个仓库的代码
   2. 将B项目作为A项目的子模块引入`git submodule add SSH_B`
   3. 如果项目B进行了更新，需要在A的本地项目进入到B的目录中，执行pull操作，然后将A的本地在推送到远程。才能在A的远程仓库更新。（即先更新A的本地项目，再同步到A的远程）
   4. 当A项目有多个子模块时，可以在A中迭代pull来感知远程仓库（将A中的所有submodule全部pull）`git submodule foreach git pull`，然后add commit push，推送到远程。
   5. 克隆带有子模块的项目A，`git clone git@github.com:mjxn/A.git --recursive`
   6. 删除项目中的子模块B，删除字项目的三个分区，工作区`rm -rf B`，暂存区`git rm --cached B`，删除对象区只需要将之前的操作提交`git add.`、 `git commit -m “注释信息”`，最后提交到远程仓库`git push`
7. subtree
   1. 将一个项目作为另一个项目的子模块，一般用与一个大项目的主工程与子模块之间的关系
   2. 在本地父工程当中给子工程起个别名，`git remote add subtree-origin git@github.com:mjxn/subtree.git`，将子工程的master分支加入到父工程中，`git subtree add -P subtree(subtree为自定义的子工程名) subtree-origin master`，或者`git subtree add -P subtree2 subtree-origin master --squash`，合并commit，为了防止子工程干扰父工程，操作的时候像减少了提交次数，实际上增加了两次，将之前的合并为一次，并且往前多提交一次，看起来好像减少了提交的次数），加了squash容易产生冲突，如果第一次加了该参数，以后每次（以`git subtree`开头的命令）都加参数，否则都不加。
   3. 通过subtree添加的子工程，远程仓库中在A项目查看模块，依然会停留在A项目，即将B项目完全引用到了A项目中，而submodule则是相当于引用，访问B时会跳转到B仓库中。
   4. 将远程仓库的子项目更新到本地项目的父工程中`git subtree pull -P subtree1 subtree-origin master`
   5. 将本地项目更新到远程父仓库中`git push`，将本地项目中的子项目更新到远程子仓库中`git subtree push` -P subtree1 subtree-origin master`
   6. `git subtree`开头的命令一般都在父工程的目录下使用
8. cherry pick
   1. 在一个分支当中将已经提交的部分转移到另一个分支（实际是复制操作），每次只能转移一个提交点，内容可以被复制，sha1值回发生改变，复制完成后，删除原有的提交点
   2. 在需要复制到的目标分支执行`git cherry-pick sha1值`（一次复制一个提交点）
   3. 在复制的时候，不要跨节点复制，否则会产生冲突
9. rebase（变基，衍合）
   1. 改变分支的根基，从B转到A，cherry-pick在A中操作，rebase在B中操作`git rebase master`
   2. rebase会改变历史提交线路
   3. rebase之后提交线路会变成一条直线
   4. sha1值和之前的不同，内容相同
   5. 比较容易产生冲突，解决后，`git add .`，`git rebase --continue`，或者放弃rebase所在分支`git rebase --skip`，放弃后要重新改变根基
   6. 还原rebase之前的场景`git rebase --abort`
   7. 建议rebase分支只在本地操作，不要推送到远程仓库；不要在master分支上进行rebase操作

## 十、团队协作开发

1. github该项目中增加合作者（全名或者邮箱）
2. 发送邀请链接
3. 接受邀请，之后就可以正常对项目进行操作

## 十一、Git GUI

### 1. gui

1. unstaged changes：工作区
2. staged changes：暂存区

### 2. gitk

1. 通过parent：找到上一次提交点
2. 通过child：找到下一次提交点

### 3. github desktop
1. github desktop [下载](https://desktop.github.com)
2. 详见[doc](https://help.github.com/en/desktop)

<div style="text-align: right;position: fixed;right: 10px; bottom: 20px;">
   <a href="./README.md">回到readme</a></br>
   <a href="#top">回到顶部</a>
</div>
