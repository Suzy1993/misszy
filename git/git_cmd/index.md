[<< 回到主页](http://suzy1993.github.io/misszy/)

## git 常用命令

### 1 基本命令
#### **1.1  查看命令帮助**
```
git help <命令名>
```

#### **1.2  配置版本库**
```
git config--global user.name "<用户名>"
git config--global user.email <邮箱>
```

#### **1.3  克隆版本库**
```
git clone <远程主机地址> #从远程主机克隆一个版本库，在本地生成一个目录，与远程主机的库同名
git clone <远程主机地址> <目录名> #从远程主机克隆一个版本库，在本地生成一个指定目录
git clone -b <远程分支名> <远程主机地址> <目录名> #从远程主机的指定分支克隆一个版本库，在本地生成一个指定目录
```

#### **1.4  新建版本库**
```
git init #在当前目录新建一个版本库
git init <目录名> #新建一个目录，并将其初始化为一个版本库
```

#### **1.5  查看工作区状态**
```
git status
```

#### **1.6  添加工作区文件到暂存区**
```
git add <文件名> #添加工作区的指定文件到暂存区
git add <目录名> #添加工作区的指定目录的所有修改过的文件到暂存区
git add . #添加工作区所有修改过的文件到暂存区
```

#### **1.7  提交暂存区文件到版本库**
```
git commit -m "注释" #提交暂存区的文件到版本库
git commit -a #git add .和git commit的结合提交工作区自上次commit之后的所有修改到版本库
git commit –am “注释" #git add和git commit –m "注释"的结合，确保所有变化都要提交才可以用，只需部分提交请用git add和git commit –m "注释"
git commit --amend -m "新注释” #使用新的一次提交替代上一次提交，会无视上一次提交，重新提交一次，既可以对上次提交的内容进行修改，也可以修改提交注释（若内容没有变化，则用于修改提交注释）
git commit --amend <文件名1> <文件名2> #重做上一次commit，并包括指定文件的新变化
```
注意：git ci 是 git commit的别名

#### **1.8  替换/撤销操作**
```
* git reset <文件名> #将暂存区中的指定文件回退到工作区，只是回退暂存区的修改，工作区不变，即撤销add操作
* git reset HEAD  <文件名> #将暂存区中的指定文件回退到工作区，只是回退暂存区的修改，工作区不变，即撤销add操作
* git reset --hard #彻底回退到某个版本，重置暂存区与工作区，与上一次commit保持一致，此命令慎用！
* git reset --soft #暂存区与工作区不作任何改变，与只是回到了git commit前的状态，即取消了commit的操作，执行git commit即可提交修改
* git reset --hard HEAD^ #彻底回退到某个版本，重置暂存区与工作区，与上一次commit保持一致
* git reset --soft HEAD^ #暂存区与工作区不作任何改变，与只是回到了git commit前的状态，即取消了commit的操作，执行git commit即可提交修改
* git reset --hard HEAD~n #彻底回退到某个版本，重置暂存区与工作区，与上n次commit保持一致
* git reset HEAD^ # 撤销提交，回退到上一个版本，HEAD指向上一个版本
* git revert HEAD^ # 撤销提交到上一个版本提交后的状态，并重新提交，HEAD指向下一个版本
* git reset HEAD~n # 撤销提交，回退到上n个版本，HEAD指向上n个版本
* git revert HEAD~n # 撤销提交到上n个版本提交后的状态，并重新提交，HEAD指向下一个版本
* git checkout . #放弃工作区的所有文件的修改
* git checkout <文件名> #放弃工作区的指定文件的修改
* git checkout head <文件名> #用head指向的版本库替换暂存区和工作区的文件，回退暂存区和工作区的修改
* git checkout -- <文件名或目录名> #放弃工作区的修改，还原指定文件或目录为当前版本库中的状态
```
注意：git co是git checkout的别名；git reset --hard不能对具体某文件进行操作。
git revert 和 git reset的区别：
* git revert是用一次新的commit来回滚之前的commit，git reset是直接删除之前的commit
* git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要revert的内容

#### **1.9  文件操作**
```
git rm <文件名> #删除版本库的文件，工作区也不再使用
git rm --cached <文件名> #删除版本库的文件，工作区还需要再使用，只是该文件不再受版本控制
git mv <文件名1> <文件名2> #移动或重命名版本库的文件
```

#### **1.10  查看代码改动**
```
git diff #查看工作区与暂存区的所有文件差异，即还没有添加到暂存区的工作区所有文件修改，这条命令所显示的内容都会在执行git add .时被添加到暂存区
git diff <文件名> #查看工作区与暂存区的指定文件差异，即还没有添加到暂存区的工作区指定文件修改，这条命令所显示的内容都会在执行git add <文件名>时被添加到暂存区
git diff --cached #查看暂存区与最后一次本地提交的文件差异，这条命令所显示的内容都会在执行git commit -m “注释"时被提交到版本库
git diff HEAD #查看工作区与最后一次本地提交的文件差异，这条命令所显示的内容都会在执行git commit -a时被提交到版本库
git diff origin #查看工作区与版本库原始版本比较
```

#### **1.11  推送到远程库**
git push的一般形式为：git push <远程主机名> <本地分支名>:<远程分支名>
```
git push origin master #若远程分支名省略，如git push origin master，则表示将本地分支推送到与之存在追踪关系的远程分支（通常二者同名），若该远程分支不存在，则会新建（对新建远程仓库的第一次推送，需要指定主分支名master，也可以设置-u参数）
git push origin :dev #若本地分支名省略，如git push origin :dev，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，等同于 git push origin --delete dev
git push origin #若当前分支和远程分支存在追踪关系，则本地分支和远程分支都可以省略，如git push origin，表示将当前分支推送到origin主机的对应分支
git push #若当前分支只有一个追踪分支，则远程主机名都可以省略，如git push，不带任何参数的git push，默认只推送当前分支
git push –u origin local #若当前分支和多个远程主机存在追踪关系，则可以使用-u参数指定一个默认主机，这样后面就可以直接使用git push而不带任何参数
```

#### **1.12  本地分支与远程分支建立追踪关系**
在某些场合，git会自动在本地分支与远程分支之间建立追踪关系，如在git clone时，所有本地分支默认与远程主机的同名分支建立追踪关系。git也允许手动建立追踪关系。
```
git branch --set-upstream master origin/dev #指定master分支追踪origin/dev分支
```

#### **1.13  取回远程主机的更新并与本地分支合并**
git pull的一般形式为：git pull <远程主机名> <远程分支名>:<本地分支名>
```
git pull origin dev:master #取回远程主机origin的dev分支的更新，再与本地分支master合并
git pull origin dev #若远程分支是与当前分支合并，则本地分支名可以省略，取回远程主机origin的dev分支的更新，再与本地当前分支合并，这等同于先做git fetch origin，再进行git merge origin/dev
git pull --rebase origin dev #若远程分支是与当前分支合并，则本地分支名可以省略，取回远程主机origin的dev分支的更新，再与本地当前分支合并，这等同于先做git fetch origin，再进行git rebase origin/dev
git pull origin #若当前分支与远程分支存在追踪关系，则本地分支名和远程分支名都可以省略，取回远程主机origin的追踪分支的更新，再与本地当前分支合并
git pull #若当前分支只有一个追踪分支，则远程主机名都可以省略，取回唯一追踪分支的更新，再与本地当前分支合并
```

#### **1.14  将远程主机的更新取回本地**
git fetch的一般形式为：git fetch <远程主机名> <远程分支名>
```
git fetch origin master #将远程主机origin的master分支的更新取回本地
git fetch origin #将远程主机origin的所有分支的更新取回本地
```
取回远程主机的更新后，可以在其基础上进行的操作：
```
git checkout -b dev origin/master #在远程主机origin的master分支的基础上，创建dev分支，并切换到dev分支
git merge origin/master #在本地分支上合并远程主机origin的master分支
git rebase origin/master #在本地分支上合并远程主机origin的master分支
```

#### **1.15  查看提交记录**
```
git log  #查看默认格式的提交记录
git log -5 #查看最近5条提交记录
git log -p #查看提交记录并显示代码改动内容
git log -p --author=<作者名>  #查看某作者的提交记录并显示代码改动内容
git log --since=<起始时间> --until=<截止时间>  #查看某段时间的提交
git log --name-only #只显示文件名
git log --pretty=oneline #只显示一行
git log --abbrev-commit #commit_id只显示前7个字符
git log --pretty=format:%h:%s  #自定义格式
git log --graph #图形化查看
git log --stat #查看修改文件统计
```

#### **1.16  打标签**
```
git tag -a <版本号> -m "<注释>” #为指定版本打标签
git tag -d <版本号> #删除指定版本的标签
```

#### **1.17  远程管理**
```
git remote add me ssh://git@git.myGit.com/myGit.git  #添加远程主机名和主机地址
git remote set-url origin ssh://git@git.myGit.com/myGit.git #设置远程主机名和主机地址
git remote –v #查看所有远程主机名和主机地址
git remote #查看所有远程主机名
git remote show <主机名> #查看指定的远程主机的详细信息
git remote rm <主机名> #删除指定的远程主机
git remote rename <原主机名> <新主机名> #重命名指定的远程主机
```

### 2 分支和合并命令
#### **2.1  创建分支**
```
git branch dev #方法1：直接从当前分支创建dev分支
git checkout -b dev #方法2：从当前分支创建dev分支，并切换到dev分支
git co –b dev：#方法3：从当前分支创建dev分支，并切换到dev分支
git checkout -b dev master #方法4：从master分支创建dev分支，并切换到dev分支
```

#### **2.2  切换分支**
```
git checkout dev #切换到dev分支
git co dev：切换到dev分支
git checkout - # 切换到上一个分支
```

#### **2.3  合并分支**
```
git merge dev #合并dev分支到当前分支，使当前分支拥有dev分支的改动
git merge dev --squash #合并dev分支到当前分支，但将分支上的提交压缩，然后手工提交变成一次提交
git merge origin/master #合并远程主机origin的master分支到当前分支
```

#### **2.4  变基分支**
```
git rebase master #将当前分支的修改重新变基到master分支上
git rebase --on-to <new_base> <current_base> #将当前分支在<current_base>基础上的修改变基到<new_base>分支上
```
git rebase和git merge的区别：
* git merge的结果看起来像一个新的合并的提交，因为merge会将合并中修改的内容生成一个新的commit
* git rebase的结果看起来像当前分支没有经过任何合并一样，如git rebase master会把当前分支里的每个提交取消掉，并且把它们临时保存为补丁，放到".git/rebase”目录中，然后把当前分支更新为最新的merge分支，最后把保存的补丁应用到当前分支上

#### **2.5  分支管理**
```
git branch #查看所有的本地分支，带*号的是当前所在分支
git branch -r #查看所有的远程分支
git branch -a #查看所有的本地分支和远程分支，带*号的是当前所在分支
git branch -d dev #删除dev分支
git branch -D dev #强制删除dev分支
git push origin --delete rem #删除远程rem分支
git push origin :rem #删除远程分支rem
git branch --merged #查看与当前分支合并过的分支，只要合并过的分支即使删掉也不用担心
git branch --no-merged #查看与当前分支没有合并过的分支
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
