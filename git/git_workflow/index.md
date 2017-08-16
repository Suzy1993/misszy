[<< 回到主页](http://suzy1993.github.io/misszy/)

## git 工作流

### 1 常见的问题
* 为什么不能使用SVN的工作流来使用git?
* git的分支？团队多人如何协作？如何解决冲突？如何进行发布？
* master（发布）、develop（主开发）、hotfix（修复）如何避免不经过验证代码上线？
* 如何在Github上与他人协作？star-fork-pull request的流程是怎样的？

### 2 工作流的本质
有效的项目流程管理和高效的开发协同约定。

### 3 git工作流
#### 3.1 集中式工作流
#### 3.1.1 git相比SVN的优势
* 每个开发都有整个工程的本地拷贝，各个开发者的开发相对独立，开发者可自由提交到自己的本地仓库，待合适时候再反馈修改到中央仓库。
* git提供了强壮的分支与合并模型。

#### 3.1.2 git分支
git缺省的分支是master，所有修改提交到master分支上，集中式工作流只用到master这一个分支。

#### 3.1.3 工作方式
* clone中央仓库
* 工作区编辑文件
* 暂存文件到暂存区
* 提交文件到本地仓库
* 本地修改推送到中央仓库

#### 3.1.4 冲突解决
若本地仓库与中央仓库有冲突，git会拒绝push。因此，在push前先从中央仓库fetch最新版本，在rebase到中央仓库上。若不同的开发者对同一文件进行了修改，rebase过程也可能有冲突，此时需要手动解决冲突，Git会暂停rebase过程，解决冲突后再继续rebase过程，若解决冲突遇到问题，可以中止整个rebase操作，重来一次。

#### 3.1.5 冲突解决
* git拒绝push：

```
git push origin master
```
* 拉取最新版本并rebase：

```
git pull --rebase origin master
```
* 可能情况1——rebase过程中出现冲突，手动编辑文件解决冲突后暂存到暂存区后继续rebase过程，再将修改推送到中央仓库：

```
git add <file>
git rebase –continue
git push origin master
```
* 可能情况2——解决冲突出现问题，中止整个rebase过程，回到git pull --rebase origin master之前的状态：

```
git rebase —abort
```

#### 3.2 功能分支工作流
功能分支工作流以集中式工作流为基础，不同的是它为各个功能分配一个专门的分支来开发，可以在把新功能集成到正式项目前，用Pull Requests的方式讨论变更。
#### 3.2.1 工作方式
功能分支工作流仍使用中央仓库，仍使用master代表正式项目，但不是直接提交修改到本地master分支，而是在开发之前为新功能创建一个新分支，注意为新分支起一个语义化的名字。
在中央仓库上存多个功能分支不会有任何问题，功能分支可以且应该push到中央仓库中，以便和其它开发者分享新的变更。

#### 3.2.2 Pull Request
某个开发完成一个功能后，不是立即合并到master，而是push到中央仓库的功能分支上并发起一个Pull Request请求去合并修改到master。 这使得，合并修改到master前，其它的开发者能Review变更。
一旦Pull Request被接受了，发布功能要做的就和集中式工作流就很像了。 首先，确定本地的master分支和上游的master分支是同步的。然后合并功能分支到本地master分支并push已经更新的本地master分支到中央仓库。
Bitbucket或Stash等仓库管理产品支持Pull Requests。

#### 3.2.3 实例
* 在开发新功能前创建一个新的分支Feature-4677：

```
git checkout -b Feature-4677 master
```
* 开发完毕后暂存并提交到本地仓库，再推送给中央仓库：

```
git add <file>
git commit –m "注释"
git push -u origin FeatureF-4677
```
-u选项设置本地分支跟踪远程分支，设置好跟踪的分支后，就可以直接使用git push命令，不用指定本地分支和远程分支的参数。
* 在Bitbucket或Stash等仓库管理产品中发起Pull Request请求合并修改到master，Code review的过程中可以迭代add、commit、push的流程，直到Pull Request被接受
* 合并修改到master

```
git checkout master
git pull
git pull origin Feature-4766
git push
```
首先切换到master分支并拉取最新的master分支，然后执行git pull origin Feature-4677合并Feature-4677分支到最新的 master分支（这一步也可以使用git merge Feature-4677命令，但git pull origin命令可以保证总是最新的新功能分支），最后将更新的master分支推送到origin。

#### 3.3 GitFlow工作流
Gitflow工作流定义了一个围绕项目发布的严格分支模型，虽比功能分支工作流复杂，但通过为功能开发、发布准备和维护分配独立的分支，提供了用于一个健壮的用于管理大型项目的框架。
#### 3.3.1 工作方式
Gitflow工作流仍使用中央仓库。和其它工作流一样，开发者在本地开发并push到中央仓库中。

#### 3.3.2 历史分支
Gitflow工作流使用2个分支来记录项目的历史：master分支存储正式发布的历史，而develop分支作为功能的集成分支。

#### 3.3.3 功能分支
每个新功能有一个自己的分支，但功能分支不是从master分支上拉出的，而是使用develop分支作为父分支。当新功能完成时，合并回develop分支。新功能分支不直接与master分支交互。

#### 3.3.4 发布分支
临近发布，从develop分支上拉出一个发布分支，用于发布，所以新功能不能再加到这个分支上，这个分支只应该做Bug修复、文档生成和其它发布任务。发布完成后，发布分支合并到master分支并分配一个版本号打好Tag。注意，发布分支所做的修改要合并回develop分支。
发布分支使得可以在完善当前发布版本的同时，可以继续开发下一个版本的功能。
发布分支命名：release-* 或 release/*

#### 3.3.5 维护分支/热修复分支：
维护分支/热修复分支用于快速给产品发布版本（production releases）打补丁，是唯一可以直接从master分支fork出来的分支。 修复完成后，合并修改到master分支和develop分支，master分支应该用新的版本号打好Tag。
为Bug修复使用专门分支，可以在不打断其它工作的情况下处理问题。可以把维护分支理解成一个直接在master分支上处理的临时发布。

#### 3.3.6 实例
* 创建develop分支，并推送到中央仓库上：

```
git branch develop
git push -u origin develop
```
* clone中央仓库，从develop分支拉出一个跟踪分支：

```
git clone <中央仓库地址>
git checkout -b develop origin/develop
```
* 从develop分支拉出一个新的功能分支：

```
git checkout -b Feature-4766 develop
```
* 新功能开发完毕后暂存并提交到本地仓库：

```
git add <file>
git commit –m "注释"
```
* 合并功能分支的修改到develop分支，删除功能分支：

```
git pull origin develop
git checkout develop
git merge Feature-4766
git push
git branch -d Feature-4766
```
* 从develop分支拉出一个发布分支：

```
git checkout -b release-0.1 develop
```
* 合并发布分支的修改到master分支和develop分支上，删除发布分支：

```
git checkout master
git merge release-0.1
git push
git checkout develop
git merge release-0.1
git push
git branch -d release-0.1
```
* 只要有合并到master分支，就为master打好Tag以方便跟踪：

```
git tag -a 0.1 -m "release-0.1" master
git push --tags
```
* 若发现bug，则从master分支拉出维护分支：

```
git checkout -b issue-#001 master
```
* 修复bug后，合并维护分支的修改到master分支和develop分支上，删除维护分支：

```
git checkout master
git merge issue-#001
git push
git checkout develop
git merge issue-#001
git push
git branch -d issue-#001
```
 
[<< 回到主页](http://suzy1993.github.io/misszy/)
