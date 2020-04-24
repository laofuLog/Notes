[toc]

# Git命令行

## 设置账号密码

`git config --global user.name "Your Name"`

`git config --global user.email "email@example.com"`

- 注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址

## 跳转盘符和文件夹

**单步跳**

`cd (盘符/文件夹名):`

**多步跳**

`cd /路径/路径/.....`

## 创建文件夹

`mkdir 文件夹名`

## 创建文件

`touch 文件名`

## 显示当前文件夹路径

`pwd`

## 创建仓库

`git init`

## 添加文件至暂存区

`git add 文件名.后缀`

## 查看库里的文件

`ls`

## 提交更改

`git commit -m "提交的日志说明"`

## 查看暂存区状态

`git status`

- 还可以告诉我们冲突的文件

## 查看修改内容

`git diff 文件名`

## 查看提交日志

`git log`

**简化日志内容**

`git log --pretty=oneline`

> log前的一大串数字就是commit id（版本号）可以用以标识每一次的修改，方便用以版本回溯或者切换版本

## 版本回退

`git reset --hard HEAD^`

- `HEAD^`指回退一个版本，`HEAD^^`指回退两个版本，`HEAD~100`指回退到前第一百个版本
- 如果是`HEAD`指的就是取消暂存区里的修改，保持当前工作区内容

## 指定跳转版本

`git reset --hard <版本号>`

> 版本号可以只写前面几个数字

## 查看所有操作过的命令

`git reflog`

## 删除文件

`rm <文件名>`

## 撤销修改

`git checkout -- <文件名>`

- 一种是文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态

- 一种是文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态

- 当文件被删除时，但是还没`commit`提交的时候，就可以用`git checkout`取消删除，还原文件

## 创建SSH键

`ssh-keygen -t rsa -C "邮箱地址"`

- 然后可以区`github`上绑定`SSH`，在设置里添加键，可以添加多个
- 为什么`GitHub`需要`SSH Key`呢？因为`GitHub`需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而`Git`支持`SSH`协议，所以，`GitHub`只要知道了你的公钥，就可以确认只有你自己才能推送

## 添加远程库

- 先在`github`上创建一个同名库

`git remote add <name(origin)> git@github.com:自己的github名/仓库名.git`

- 添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库

## 将本地库内容推送至远程库上

`git push -u origin master`

- 把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程

- 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令

**提交内容至远程库**

`git push origin master`

## 从Github上克隆库到本地

`git clone git@github.com:Git用户名/仓库名.git`

- `GitHub`给出的地址不止一个，还可用`https://github.com/michaelliao/gitskills.git`这样的地址，实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议，使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh`协议而只能用`https`。

## 删除远程库

`git remote rm <origin-name>`

## 创建及切换分支

`git checkout -b 分支名`

- 上述命令相当于两条命令：
  - 创建分支：`git branch 分支名`
  - 切换分支：`git checkout 分支名`

- `Git`提供新的创建切换分支，比`checkout`更容易理解，因为`checkout`还有撤销操作的意思
  - 创建切换分支：`git switch -c 分支名`
  - 切换分支：`git switch 分支名`

## 查看分支

`git branch`

## 合并分支到主干

`git merge 分支名`

## 删除分支

`git branch -d 分支名`

## 解决冲突

- 当分支与主干修改同一个内容，并提交，之后合并的时候就会有冲突

- `Git`用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容
- 需要手动看下冲突内容，如何取舍

## 查看分支合并图

`git log --graph`

简化输出内容：`git log --graph --pretty=oneline --abbrev-commit`

## 使用--no-ff方式合并分支

- `Git`默认使用`Fast forward`模式合并分支，但这种模式下，删除分支后，会丢掉分支信息
- 如果要强制禁用`Fast forward`模式，`Git`就会在`merge`时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息

`git merge --no-ff -m "日志消息" 分支名`

- 因为本次合并要创建一个新的`commit`，所以加上`-m`参数，把`commit`描述写进去

## 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理

- 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活

- 干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本

- 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了

- 所以，团队合作的分支看起来就像这样

  ![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

## 修改bug步骤

1. 保存当前分支内容		`git stash`

2. 切换分支或者就在本分支     `git switch <name>`

3. 创建切换新分支       `git switch -c <name>`

4. 修改`bug`

5. 切换回要修改`bug`的分支

6. 合并分支       `git merhe --no-ff -m <message> <name>`

7. 删除新分支    `git branch -d <name>`

8. 切换回正在工作的分支

9. 同步修改的`bug`到当前分支 `git cherry-pick <commit id>`

10. 还原之前保存的内容  `git stash pop`  或者`git stash apply`

    > 但是使用apply不会删除stash里的内容，需要使用`git stash drop`来删除

## 查看Stash里保存的内容

`git stash list`

## 开发新功能

1. 创建新分支
2. 开发功能
3. 提交新内容
4. 切换回分支
5. 合并分支
6. 删除新分支

## 强制删除分支

`git branch -D <name>`

## 关联远程分支

`git branch --set-upstream-to <branch-name> <origin(repote-name)>/<branch-name>`

##　抓取分支内容

`git pull`

## 把本地未push的分叉提交历史整理成直线

`git rebase`

- `rebase`的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比

## 创建标签

`git tag <tag-name(v1.0)>`

- 给之前的`commit`创建标签

  `git tag <tag-name> <commit id>`

- 默认标签是打在最新提交的`commit`上的
- 标签总是和某个`commit`挂钩。如果这个`commit`既出现在`master`分支，又出现在`dev`分支，那么在这两个分支上都可以看到这个标签

## 查看所有标签

`git tag`

## 查看标签信息

`git show <tag-name>`

**查看标签详细信息**

`git tag -a <tag-name> -m "<message>"  (<commit id>)`

- `-a`是指定标签名
- `-m`指说明文字

## 推送标签至远程库

**推送单个标签**

`git push <origin-name> <tag-name>`

**整体推送**

`git push <origin-name> --tags`

## 删除本地标签

`git tag -d <tag-name>`

## 删除远程标签

1. 先把本地的删除
2. `git push origin:refs/tags/<tag-name>`

## 配置命令别名

`git config --global alias 别名 <原命令>`

- 配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用
- 每个仓库的Git配置文件都放在`.git/config`文件中

## 忽略特殊文件

1. `vim .gitignore`创建`gitignore`文件

2. 添加忽略信息

   ```csharp
   //注释
   #.......
   
   //过滤文件设置，表示过滤这个文件夹
   /target/ 
   
   //表示过滤某种类型的文件
   *.mdb  ，*.ldb  ，*.sln 
   //表示指定过滤某个文件下具体文件
   /mtk/do.c ，/mtk/if.h
   // !开头表示不过滤
   !*.c , !/dir/subdir/    
   //支持通配符：过滤repo中所有以.o或者.a为扩展名的文件
   *.[oa]  
       
   ------------------------------------------------
   //例如
   # Windows:
   Thumbs.db
   ehthumbs.db
   Desktop.ini
   
   # Python:
   *.py[cod]
   *.so
   *.egg
   *.egg-info
   dist
   build
   
   # My configurations:
   db.ini
   deploy_key_rsa  
   ```

3. 退出`vim`模式

现在就可以忽略这些你配置的文件

## Vim编辑器

### 进入vim编辑器

`vim <filspath>`

- 可以创建一个文件并编辑，或者直接对一个文件进行编辑

### 编辑器的三种模式

- 接受、执行 `vim`操作命令的模式，打开文件后的默认模式
- 编辑模式：对打开的文件内容进行 增、删、改 操作的模式

- 在编辑模式下按下`ESC`键，回退到命令模式；在命令模式下按`i`，进入编辑模式

### 命令行模式下输入命令

`shift + ;`

### 常用命令

- `q!` 强制退出不保存
- `q`退出不保存
- `wq!`强制退出并保存，不加`!`就是不强制
- `w`保存编辑后的内容
- `w!`强制写入文件，当一些文件有写入权限时可以使用
- 上述命令后面加上文件名，就是将内容写入指定文件
- `ZZ`使用`ZZ`命令时，如果文件已经做过编辑处理，则把内存缓冲区中的数据写到启动`vim`时指定的文件中，然后退出`vim`编辑器。否则只是退出`vim`而已。注意，`ZZ`命令前面无需加冒号`“：”`，也无需按`Enter`键

## 参考资料

[Git教程](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)