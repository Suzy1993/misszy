[<< 回到主页](http://suzy1993.github.io/misszy/)

## git merge VS git rebase

git merge和git rebase都是用来合并分支的。

### 1 实例
* 基于远程分支origin，创建mywork分支：git checkout -b mywork origin
* 假设origin已有2个提交，在mywork分支做了一些修改并生成2个提交，与此同时，其他人也在origin分支上做了一些修改并生成2个提交。

![image](images/1.png)

### 2 git merge origin
![image](images/2.png)

### 3 git rebase origin
![image](images/3.png)

### 4 git rebase和git merge的区别
* git merge的结果看起来像一个新的合并的提交，因为merge会将合并中修改的内容生成一个新的 commit。
* git rebase的结果看起来像当前分支没有经过任何合并一样，如git rebase master会把当前分支里的每个提交取消掉，并且把它们临时保存为补丁，放到".git/rebase”目录中，然后把当前分支更新为最新的merge分支，最后把保存的补丁应用到当前分支上。
用git log查看commit时， commit的顺序不同：
* 假设提交时间：C3<C5<C4<C6。
* 使用git merge合并，commit的顺序（从新到旧）是：C7->C6->C4->C5->C3->C2->C1。
* 使用git rebase合并，commit的顺序（从新到旧）是：C7->C6'->C5'->C4->C3->C2->C1。
* 其中，C6'提交是C6提交的克隆，C5'提交只C5提交的克隆。
* 从用户的角度看，使用git merge合并后所看到的commit的顺序（从新到旧）是：C7->C6->C4->C5->C3->C2->C1。
* 从用户的角度看，使用git rebase合并后所看到的commit的顺序（从新到旧）是：C7->C6->C5->C4->C3->C2->C1。

[<< 回到主页](http://suzy1993.github.io/misszy/)
