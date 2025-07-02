# Git 命令快速参考

## 🚀 协作基础命令

### 仓库操作
```bash
# 克隆仓库
git clone <repository-url>

# 添加远程仓库
git remote add upstream <original-repo-url>

# 查看远程仓库
git remote -v

# 获取远程更新
git fetch origin
git fetch upstream
```

### 分支管理
```bash
# 查看分支
git branch              # 本地分支
git branch -r           # 远程分支
git branch -a           # 所有分支

# 创建并切换分支
git checkout -b feature/new-feature

# 切换分支
git checkout main
git checkout feature/new-feature

# 删除分支
git branch -d feature/old-feature      # 安全删除
git branch -D feature/old-feature      # 强制删除
git push origin --delete feature/old-feature  # 删除远程分支
```

## 🔄 同步和合并

### 保持分支更新
```bash
# 同步主分支
git checkout main
git pull upstream main
git push origin main

# 更新功能分支
git checkout feature/my-feature
git merge main
# 或使用 rebase
git rebase main
```

### 解决冲突
```bash
# 查看冲突状态
git status

# 查看冲突详情
git diff

# 标记冲突已解决
git add <conflicted-file>

# 完成合并
git commit

# 放弃合并
git merge --abort
```

## 📝 提交管理

### 基本提交
```bash
# 查看状态
git status

# 添加文件
git add .                    # 添加所有
git add <file>              # 添加特定文件
git add -p                  # 交互式添加

# 提交
git commit -m "feat: add new feature"

# 修改最后一次提交
git commit --amend
```

### 提交历史
```bash
# 查看历史
git log --oneline           # 简洁格式
git log --graph            # 图形化显示
git log --author="name"    # 按作者筛选

# 查看文件变更
git show <commit-hash>
git diff HEAD~1            # 与上次提交比较
```

## 🔍 信息查看

### 状态检查
```bash
# 工作区状态
git status

# 查看差异
git diff                   # 工作区与暂存区
git diff --staged         # 暂存区与上次提交
git diff HEAD~1           # 与指定提交比较

# 查看远程分支信息
git remote show origin
```

### 历史和日志
```bash
# 美化的历史记录
git log --oneline --graph --decorate --all

# 查看特定文件的历史
git log --follow <file>

# 查看谁修改了什么
git blame <file>

# 搜索提交信息
git log --grep="keyword"
```

## 🛠️ 高级操作

### Stash（暂存）
```bash
# 暂存当前工作
git stash
git stash save "work in progress"

# 查看暂存列表
git stash list

# 恢复暂存
git stash pop              # 恢复最新的并删除
git stash apply            # 恢复但保留暂存
git stash apply stash@{1}  # 恢复特定暂存

# 删除暂存
git stash drop
git stash clear            # 清空所有暂存
```

### Reset 和 Revert
```bash
# 撤销工作区更改
git checkout -- <file>
git restore <file>         # Git 2.23+ 新命令

# 撤销暂存
git reset HEAD <file>
git restore --staged <file>

# 重置到指定提交
git reset --soft HEAD~1    # 保留更改在暂存区
git reset --mixed HEAD~1   # 保留更改在工作区（默认）
git reset --hard HEAD~1    # 完全删除更改

# 安全的撤销提交
git revert <commit-hash>   # 创建新提交来撤销
```

### Rebase 操作
```bash
# 基础 rebase
git rebase main

# 交互式 rebase
git rebase -i HEAD~3

# 在交互式 rebase 中：
# pick   = 使用提交
# reword = 使用提交但编辑提交信息
# edit   = 使用提交但停下来修改
# squash = 使用提交但合并到前一个提交
# drop   = 删除提交

# 继续 rebase
git rebase --continue

# 中止 rebase
git rebase --abort
```

## 🏷️ 标签管理
```bash
# 创建标签
git tag v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"

# 查看标签
git tag
git show v1.0.0

# 推送标签
git push origin v1.0.0
git push origin --tags      # 推送所有标签

# 删除标签
git tag -d v1.0.0           # 本地删除
git push origin --delete v1.0.0  # 远程删除
```

## 🔧 配置和设置

### 全局配置
```bash
# 设置用户信息
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 设置默认分支名
git config --global init.defaultBranch main

# 设置默认编辑器
git config --global core.editor "code --wait"

# 查看配置
git config --list
git config user.name
```

### 别名设置
```bash
# 常用别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"
```

## 🚨 紧急情况处理

### 误操作恢复
```bash
# 查看引用日志
git reflog

# 恢复到指定状态
git reset --hard HEAD@{2}

# 恢复已删除的分支
git checkout -b recovered-branch <commit-hash>
```

### 清理操作
```bash
# 清理未跟踪的文件
git clean -n               # 预览要删除的文件
git clean -f               # 删除文件
git clean -fd              # 删除文件和目录

# 修剪远程分支引用
git remote prune origin
```

## 📊 常用组合命令

### 完整工作流
```bash
# 开始新功能
git checkout main
git pull upstream main
git checkout -b feature/new-feature

# 开发和提交
# ... 编写代码 ...
git add .
git commit -m "feat: implement new feature"

# 推送并创建 PR
git push -u origin feature/new-feature

# 清理（PR 合并后）
git checkout main
git pull upstream main
git branch -d feature/new-feature
```

### 同步 Fork
```bash
# 完整同步流程
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

💡 **提示**: 将常用命令设置为别名可以大大提高效率！ 