### 常用命令
```
# 配置登录用户
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
git init                    # 初始化本地git仓库（创建新仓库）
git add .                   # 增加当前子目录下所有更改过的文件至index
git add xyz.txt abc.txt     # 添加xyz文件至index
git commit -m 'xxx'         # 提交
git status                  # 查看仓库状态
git diff                    # 查看修改内容，默认为工作区和暂存区的差异 
                              main 当前分支和main分支的差异 
                              -- aa.txt 比较aa.txt文件的变化
# Git版本管理
git log                     # 显示提交日志，--pretty=oneline 参数可以变得更简洁
git reflog                  # 显示命令日志
git reset --hard HEAD^      # 回退上一个版本，参数有--hard 回退上个版本提交状态 
                              --soft 回退上个版本未提交状态 
                              --mixed 回退上个版本已添加但未提交状态
git reset -- hard <commit id> # 回退到指定版本号
git rm <file>               # 删除文件
# Git分支管理
git clone [url]                     # clone远程仓库
git push origin master              # 将当前分支push到远程master分支
git push -u origin master           # 首次推送会关联本地master和远程master分支
git branch                          # 显示本地分支情况
git branch master_copy              # 创建新分支
git branch -d master_copy           # 删除分支
                                      删除未合并的分支需要使用参数 -D
git checkout master_copy            # 切换到master_copy分支
git checkout -b master_copy         # 从当前分支创建新分支master_copy并切换
git checkout -b master master_copy  # 完整版创建切换分支
git stash                           # 暂存工作区
git stash list                      # 查看暂存列表
git stash apply stashxxx            # 恢复某个暂存区
git stash drop                      # 删除某个暂存区
git stash pop                       # 恢复的同时删除暂存区
git cherry-pick [commit id]         # 复制特定的提交到当前分支，比如修了某个点但又不想merge

git merge origin/master             # 合并远程master分支至当前分支
git pull                            # 拉取最新代码
git rebase                          # 变基操作，将分叉的提交历史变为直线

```

### 更多命令

```
git commit --amend -m 'xxx'                               # 合并上一次提交（用于反复修改）
git commit -am 'xxx'                                      # 将add和commit合为一步
git branch -d hotfixes/BJVEP933                           # 删除分支hotfixes/BJVEP933（本分支修改已合并到其他分支）
git pull origin master                                    # 获取远程分支master并merge到当前分支
git fetch                                                 # 获取所有远程分支（不更新本地分支，另需merge）
git branch -a                                             # 显示所有分支
git branch -m master master_copy                          # 本地分支改名
git revert dfb02e6e4f2f7b573337763e5c0013802e392818       # 撤销提交dfb02e6e4f2f7b573337763e5c0013802e392818
git show-branch --all                                     # 图示所有分支历史
git whatchanged                                           # 显示提交历史对应的文件修改
git log --stat                                            # 显示提交日志及相关变动文件
git log --graph                                           # 查看分支合并图
git show dfb02e6e4f2f7b573337763e5c0013802e392818         # 显示某个提交的详细内容
git show dfb02                                            # 可只用commitid的前几位
git show HEAD                                             # 显示HEAD提交日志
git tag                                                   # 显示已存在的tag
git tag -a v2.0 -m 'xxx'                                  # 增加v2.0的tag
git show v2.0                                             # 显示v2.0的日志及详细内容
git log v2.0                                              # 显示v2.0的日志
git diff --cached                                         # 显示所有已添加index但还未commit的变更
git diff HEAD^                                            # 比较与上一个版本的差异
git diff HEAD -- ./lib                                    # 比较与HEAD版本lib目录的差异
git diff origin/master..master                            # 比较远程分支master上有本地分支master上没有的
git diff origin/master..master --stat                     # 只显示差异的文件，不显示具体内容
git remote add origin git+ssh://git@192.168.53.168/VT.git # 增加远程定义（用于push/pull/fetch）
git branch --contains 50089                               # 显示包含提交50089的分支
git branch -r                                             # 显示所有原创分支
git branch --merged                                       # 显示所有已合并到当前分支的分支
git branch --no-merged                                    # 显示所有未合并到当前分支的分支
git config core.ignorecase          # 检查是否大小写敏感, ture为敏感
git config core.ignorecase false    # 设置为对大小写敏感
```
单引号：直接保留内容，不允许变量展开；双引号：部分符号需要转义，允许变量展开  
但对于git指令没有区别


### 背景
IDEA的图形化界面能很便捷地使用Git，但仍无法完全满足需要  
(1) 难以保持commit简洁。压缩commit仅限于本地且不能压缩已经push的部分，项目大多数时候需要先push在云端跑起来debug后再做修改  
(2) 解决分支冲突出现异常时无法回退，只能先删除本地分支再拉分支，这样会丢掉新的提交记录   
此外，如果切换IDE图形化界面会需要适应的时间

### Git
**1.关于Git**  
集中式版本控制系统：所有代码统一提交到中央服务器，必须联网   
分布式版本控制系统：每台电脑都存储了完整的版本库，无需联网，Git属于此类

**2.Git初始化**  
① 首先使用`git config --global ...`命令设置用户  
② 使用`git init`命令将当前目录设置成Git管理仓库，当前目录会新增一个`.git`文件  
③ 将修改提交到暂存区 `git add aaa.txt bbb.txt`，暂存区位于`.git`文件中  
④ 从暂存区提交代码 `git commit -m "first commit"`  
注意执行命令的目录，Git提交和分支操作只在仓库目录中有效

**3.Git版本管理**   
3.1 回退版本
执行`git log`可以显示commit和commit id (版本号)
```
63df45081baf83b02a49a0f98fe4568856be4cb0 (HEAD -> main) version 3
d636a57e551dbdf96b2655a8de25922e771f12a1 version 2
c11b7d4bca30fc347e3eeb9640d7877a8cc02b13 version 1
3b1127ae4e31de575c1f176fbc91b3f5289dd223 (origin/main) first commit

```

回退上一个版本使用指令`git reset --hard HEAD^`   
指定回退某一版本`git reset --hard 63df4`   

其中，`--hard`会回退到上个版本的已提交状态, `--hard`会回退到上个版本的已提交状态,`--soft`会回退到上个版本的未提交状态, `--mixed`会回退到上个版本已添加但未提交的状态。版本回退之后log中就看不到新版本状态了   

此时使用`git reflog`查看命令中的版本号来切换版本
```
c11b7d4 (HEAD -> main) HEAD@{0}: reset: moving to c11b7d
63df450 HEAD@{1}: commit: version 3
d636a57 HEAD@{2}: commit: version2
c11b7d4 (HEAD -> main) HEAD@{3}: commit: version 1
3b1127a (origin/main) HEAD@{4}: commit (initial): first commit
```

3.2 撤销修改和删除文件  
使用`git restore <文件>`命令，可以撤销该文件在工作区的全部修改，回退到暂存区中的版本

删除文件先rm删，提交之前恢复删除的文件使用`git checkout -- <文件>`或者`git restore <文件>`。使用`git rm <文件>`删除相当于删除之后直接提交，此时无法使用上述指令恢复  
文件删除提交到暂存区之后就只能使用log/reflog恢复了  


**4.Git分支管理**  
常说的分支其实是指针，创建分支其实是创建新的指针  
查看分支状态 `git branch`  

4.1 分支切换流程   
① 创建并切换新分支：`git checkout -b newbranch`   
 `-b`表示切换，实际等同于`git branch newbranch` + `git checkout newbranch`  
② 在新分支上修改提交  
③ 切换到master分支 `git checkout master`   
  合并分支到master `git merge newbranch`  
④ 删除新分支 `git branch -d newbranch`  

4.2 分支管理策略  
使用Fast forward模式在删除分支后可能丢掉分支信息，使用`git log`可查看分支历史    
禁用该模式使用参数`--no-ff`:  `git merge --no-ff -m 'test merge' newbranch`  

4.3 暂存工作现场
① 使用`git stash` 暂存工作现场
② 切换分支开发  
③ 使用`git stash list`查看暂存，`git stash apply xxx`恢复工作现场，使用`git stash drop`删除暂存  
  或者直接使用`git stash pop`恢复的同时删除暂存

4.4 多人协作   
`git remote` 查看远程仓库信息，如果需要查看详细信息加上`-v`  
`git clone` 把远程仓库拉到本地     
① 先commit修改代码，注意不能直接拉代码会造成本地记录丢失  
② 拉新纪录`git pull`并解决冲突  
③ `git push origin xxxbranch` 推送本地分支到远程  



// Todo
### IDEA-Git功能
### VSCode-Git功能
### 问题
1. 提交习惯
提交到共同开发分支上的代码需要先能跑通
2. 如何合并commit



### 参考资料
[廖雪峰Git教程](https://liaoxuefeng.com/books/git/introduction/index.html) | [菜鸟教程](https://www.runoob.com/git/git-tutorial.html)