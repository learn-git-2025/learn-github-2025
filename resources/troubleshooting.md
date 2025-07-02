# 协作故障排除指南

## 🚨 常见问题和解决方案

### 1. 推送被拒绝

#### 问题
```bash
$ git push origin feature/my-feature
To github.com:user/repo.git
 ! [rejected]        feature/my-feature -> feature/my-feature (non-fast-forward)
error: failed to push some refs to 'github.com:user/repo.git'
```

#### 原因
- 远程分支有新的提交
- 本地分支落后于远程分支

#### 解决方案
```bash
# 方案1：先拉取再推送
git pull origin feature/my-feature
# 解决可能的冲突
git push origin feature/my-feature

# 方案2：使用 rebase
git pull --rebase origin feature/my-feature
git push origin feature/my-feature
```

### 2. 合并冲突

#### 问题
```bash
$ git merge main
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

#### 解决步骤
```bash
# 1. 查看冲突文件
git status

# 2. 编辑冲突文件，删除冲突标记并选择正确内容
# <<<<<<< HEAD
# 当前分支内容
# =======
# 要合并分支内容
# >>>>>>> branch-name

# 3. 标记冲突已解决
git add <conflicted-file>

# 4. 完成合并
git commit

# 如果想放弃合并
git merge --abort
```

### 3. 意外提交到错误分支

#### 问题
在 main 分支上直接提交了代码，应该在功能分支上提交。

#### 解决方案
```bash
# 创建新分支保存当前工作
git branch feature/accidental-commit

# 回退 main 分支
git reset --hard HEAD~1

# 切换到功能分支继续工作
git checkout feature/accidental-commit
```

### 4. 提交信息写错了

#### 问题
刚刚提交的信息有拼写错误或不够清晰。

#### 解决方案
```bash
# 修改最后一次提交信息（未推送）
git commit --amend

# 如果已经推送，需要强制推送（谨慎使用）
git commit --amend
git push --force-with-lease origin feature/my-feature
```

### 5. 不小心删除了文件

#### 问题
```bash
$ rm important-file.txt
$ git add .
$ git commit -m "cleanup"
```

#### 解决方案
```bash
# 恢复最近删除的文件
git checkout HEAD~1 -- important-file.txt

# 或从特定提交恢复
git checkout <commit-hash> -- important-file.txt

# 查看文件删除历史
git log --diff-filter=D --summary
```

### 6. 分支同步问题

#### 问题
功能分支落后主分支很多提交，导致冲突复杂。

#### 解决方案
```bash
# 方案1：定期合并主分支
git checkout feature/my-feature
git merge main
# 解决冲突并提交

# 方案2：使用 rebase 保持线性历史
git checkout feature/my-feature
git rebase main
# 逐个解决冲突
git rebase --continue
```

### 7. 工作区混乱

#### 问题
工作区有很多未提交的更改，需要切换分支。

#### 解决方案
```bash
# 暂存当前工作
git stash save "work in progress"

# 切换分支
git checkout other-branch

# 回来时恢复工作
git checkout original-branch
git stash pop
```

### 8. 远程分支删除后本地仍显示

#### 问题
```bash
$ git branch -r
  origin/feature/deleted-branch  # 这个分支已在 GitHub 上删除
  origin/main
```

#### 解决方案
```bash
# 清理已删除的远程分支引用
git remote prune origin

# 或者在拉取时自动清理
git pull --prune
```

### 9. SSH 连接问题

#### 问题
```bash
$ git push origin main
git@github.com: Permission denied (publickey).
```

#### 解决方案
```bash
# 检查 SSH 密钥
ssh -T git@github.com

# 生成新的 SSH 密钥
ssh-keygen -t ed25519 -C "your_email@example.com"

# 添加到 SSH agent
ssh-add ~/.ssh/id_ed25519

# 将公钥添加到 GitHub 账户
cat ~/.ssh/id_ed25519.pub
```

### 10. Fork 同步问题

#### 问题
Fork 的仓库落后原仓库很多提交。

#### 解决方案
```bash
# 添加上游仓库
git remote add upstream https://github.com/original-owner/repo.git

# 获取上游更新
git fetch upstream

# 合并到本地主分支
git checkout main
git merge upstream/main

# 推送到你的 Fork
git push origin main
```

## 🛠️ 预防措施

### 1. 定期同步
```bash
# 设置定期同步提醒
# 每天开始工作前
git checkout main
git pull upstream main
```

### 2. 提交前检查
```bash
# 养成习惯：提交前检查
git status
git diff --staged
git log --oneline -5
```

### 3. 使用分支保护
```bash
# 在 GitHub 上设置分支保护规则
# - 要求 PR 审查
# - 要求状态检查通过
# - 要求分支是最新的
```

## 🆘 紧急恢复

### 查看引用日志
```bash
# 查看最近的操作
git reflog

# 恢复到指定状态
git reset --hard HEAD@{2}
```

### 恢复删除的分支
```bash
# 查找分支最后的提交
git reflog | grep "checkout"

# 重新创建分支
git checkout -b recovered-branch <commit-hash>
```

### 完全重置仓库
```bash
# 警告：这会删除所有本地更改
git fetch origin
git reset --hard origin/main
git clean -fd
```

## 📞 寻求帮助

### 1. 使用 Git 内置帮助
```bash
git help <command>
git <command> --help
```

### 2. 安全的操作
```bash
# 在进行危险操作前，先备份
git branch backup-$(date +%Y%m%d-%H%M%S)

# 使用 --dry-run 预览操作
git clean --dry-run

# 使用 --force-with-lease 而不是 --force
git push --force-with-lease origin feature/my-feature
```

### 3. 团队求助
- 在 PR 中 @mention 有经验的同事
- 在团队聊天工具中寻求帮助
- 查看项目的 CONTRIBUTING.md 文档

## 🔧 调试工具

### 1. 详细输出
```bash
# 查看详细的 Git 操作
GIT_TRACE=1 git push origin main

# 查看网络相关问题
GIT_CURL_VERBOSE=1 git push origin main
```

### 2. 状态检查
```bash
# 完整的仓库状态
git status --porcelain
git log --oneline --graph --all
git remote -v
```

---

💡 **记住**: 遇到问题时不要惊慌，Git 的设计让数据丢失非常困难。大多数操作都可以撤销或恢复！ 