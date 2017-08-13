[<< 回到主页](http://suzy1993.github.io/misszy/)

## git stash

### 1 应用场景
当不想提交当前完成了一半的代码，但却不得不修改一个紧急bug，那么使用git stash就可以将当前未提交到的代码推入到Git的栈中，此时工作区和上一次提交的内容是完全一样的，可以放心的修复bug，等修复完bug，提交后，再恢复以前一半的工作。

### 2 本质
stash本质是一个伪commit，对工作区的改动进行一次快照。

### 3 命令
```
git stash：存储当前工作区
git stash list：获取当前的Git栈信息
git stash pop：恢复最新一次的stash，并在list中删除
git stash apply：恢复最新一次的stash
git stash drop：删除最新一次的stash
git stash apply stash@{<版本号>}：恢复指定的stash，最新的版本号为0
git stash drop stash@{<版本号>}：在list中删除指定的stash，最新的版本号为0
```

### 4 操作过程
```
git stash：将feature分支当前未提交到的代码推入到Git的栈中
git checkout master：假定需要在master分支上修复bug，切换到master分支
git checkout -b issue-001：从master创建临时分支issue-001，并切换到issue-001分支
修复bug
git add <文件名>
git commit -m "注释"
git checkout master：切换到master分支
git merge issue-001：合并修改到master分支
git branch -d issue-101：删除issue-101分支
git checkout feature：切换回feature分支
git stash pop：恢复工作
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
