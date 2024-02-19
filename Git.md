# Git使用

## 设置

### 查看配置信息

```shell
git config --list
```

### 设置用户名密码和邮箱

```shell
git config --global user.name "username"
git config --global user.password "password"
git config --global user.email "123@qq.com"
```

## 工作区、暂存区、本地仓库、远程仓库

### 工作区

Git仓库所在的目录

### 暂存区

.git文件保存

`git add `指令将文件添加到暂存区

### 本地仓库

使用`git commit -m`将暂存区的内容保存到本地仓库的当前分支

### 远程仓库

#### 关联远程仓库

```shell
 git remote add origin <remote uri> # origin为远程仓库的名称
```

#### 查看远程仓库

```shell
git remote -v
```

#### 解绑远程仓库

```shell
git remote rm <name>
```

#### 推送

```shell
git push <remote> <branch> # 将本地分支推动到远程分支
git push -u <remote> <branch>	# 同时关联本地分分支和远程分支		
```

#### 拉取

```shell
git pull<remote> <branch> # 等于先fetch远程分支，再merge远程分支和本地分支
```

#### 撤销修改

```shell
git reset HEAD <filename> # 将暂存区文件还原到最后一次提交的状态
git checkout -- <filename> # 将工作区文件还原成最后一次提交的状态
```

### SSH设置

1. 创建SSH Key 

   - ```shell
     ssh-keygen -t rsa -C "youremail@example.com"
     ```

     可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，`id_rsa`是私钥，id_rsa.pub`是公钥，

2. 设置公钥

   - 登录GitHub，将`id_rsa.pub`文件中的公钥设置进去

# Git文件状态

### 查看

```shell
git status
```

### 文件状态

1. 未追踪（Untracked）:
   - 文件存在于工作目录中，但尚未被Git跟踪。这可能是新创建的文件或被添加到 `.gitignore` 中的文件。
2. 已暂存（Staged）:
   - 文件已经被 `git add` 命令添加到了暂存区，准备提交到版本库。暂存区是一个缓冲区，保存了即将提交的更改。
3. 已修改（Modified）:
   - 文件在工作目录中发生了更改，但这些更改尚未被添加到暂存区。这是指已经被Git追踪的文件，但有未保存的更改。
4. 已提交（Committed）:
   - 文件的更改已经被提交到本地版本库中。这表示文件的当前状态已经保存在版本历史中。

## 版本

### HEAD指针

HEAD表示当前版本

HEAD^表示前一个版本

HEAD~2表示前2个版本

### 查看记录

```shell
git log 
git log --oneline # 一行显示
git log --graph # 查看分支图
```

### 查看命令记录

```shell
git reflog
```

### 版本回退

```shell
git reset <logId>
# 参数 
#  --hard 表示暂存区和工作区都还原
# --mixed 默认，将HEAD指向之前的记录，并重置暂存区
# --sorf 不修改暂存区和工作区
```

## 分支管理

### 分支命令

```shell
git branch <branch> # 创建分支
git switch -c <branch> # 创建并切换分支
git switch <branch># 切换分支
git branch # 查看分支
git merge <branch> # 合并分支，将对应分支的内容合并到当前分支来
git branch -d <branch>  # 删除分支
```

### 暂存内容

情景：当前正在开发某一功能，mater出现bug，当前功能未完成，需要立刻修复bug

```shell
git stash # 保存现场，无法追踪未跟踪文件
git switch master # 切换到master分支
git branch bug-001 # 创建bug分支
#####
# 修复bug
#####
git switch master # 修复完bug后切换回master
git merge --no-ff -m "fix bug 001" bug-001 # 合并
git switch dev # 切换回dev

git cherry-pick <gitHash> # 将修改bug这次提交应用到dev分支

git status # 查看保存的现场
git stash pop # 弹出保存的现场
```

## Rebase

```shell
git rebase <base-branch> # 将当前分支的提交逐个应用到对应的分支
```

