# 5. 冲突解决

合并冲突是团队协作中不可避免的情况。本节将教您如何识别、理解和有效解决各种类型的合并冲突。

## 🎯 本节目标

- 理解合并冲突的产生原因
- 学会识别不同类型的冲突
- 掌握手动解决冲突的技巧
- 了解预防冲突的最佳实践

## 💡 什么是合并冲突？

**合并冲突**发生在 Git 无法自动合并两个分支的更改时。通常是因为两个分支修改了相同文件的相同部分。

### 冲突产生的场景

```bash
# 场景1：功能分支与主分支的冲突
main branch:     A---B---C---D
                  \           
feature branch:    E---F---G   
```

如果 D 和 G 都修改了同一个文件的同一行，合并时就会产生冲突。

## 🔍 识别冲突类型

### 1. 内容冲突（最常见）
两个分支修改了同一文件的同一部分。

### 2. 删除/修改冲突
一个分支删除了文件，另一个分支修改了该文件。

### 3. 重命名冲突
两个分支都重命名了同一个文件，但名称不同。

### 4. 模式变更冲突
文件权限或类型发生冲突。

## 🛠️ 解决内容冲突

### 第一步：识别冲突

当合并发生冲突时，Git 会显示类似信息：

```bash
$ git merge feature/user-profile
Auto-merging src/user.js
CONFLICT (content): Merge conflict in src/user.js
Automatic merge failed; fix conflicts and then commit the result.
```

### 第二步：查看冲突状态

```bash
# 查看哪些文件有冲突
git status

# 预期输出
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   src/user.js
```

### 第三步：理解冲突标记

冲突文件中会包含特殊标记：

```javascript
function validateUser(user) {
<<<<<<< HEAD
  // 主分支的代码
  if (!user.email || !user.name) {
    throw new Error('Email and name are required');
  }
=======
  // 功能分支的代码  
  if (!user.email || !user.name || !user.phone) {
    throw new Error('Email, name and phone are required');
  }
>>>>>>> feature/user-profile
  
  return true;
}
```

### 冲突标记说明

```bash
<<<<<<< HEAD           # 当前分支的内容开始
这里是当前分支的代码
=======               # 分隔符
这里是要合并分支的代码  
>>>>>>> branch-name    # 要合并分支的内容结束
```

### 第四步：手动解决冲突

根据需求选择保留的代码：

#### 选项1：保留当前分支
```javascript
function validateUser(user) {
  if (!user.email || !user.name) {
    throw new Error('Email and name are required');
  }
  return true;
}
```

#### 选项2：保留目标分支
```javascript
function validateUser(user) {
  if (!user.email || !user.name || !user.phone) {
    throw new Error('Email, name and phone are required');
  }
  return true;
}
```

#### 选项3：合并两者（最常见）
```javascript
function validateUser(user) {
  // 合并逻辑，保留手机验证但优化错误信息
  if (!user.email || !user.name) {
    throw new Error('Email and name are required');
  }
  
  if (!user.phone) {
    console.warn('Phone number is recommended for better user experience');
  }
  
  return true;
}
```

### 第五步：标记冲突已解决

```bash
# 添加解决后的文件
git add src/user.js

# 检查状态
git status
# 应该显示：All conflicts fixed but you are still merging.

# 完成合并
git commit
# 或者提供自定义提交信息
git commit -m "resolve merge conflict in user validation"
```

## 🧰 工具辅助解决冲突

### 1. Git 内置工具

```bash
# 使用 mergetool 启动图形化冲突解决工具
git mergetool

# 配置默认的 merge 工具
git config --global merge.tool vimdiff
# 或者使用其他工具：kdiff3, meld, etc.
```

### 2. VS Code 冲突解决

VS Code 提供了友好的冲突解决界面：

```
文件中会显示：
┌────────────────────────────────────┐
│ Accept Current Change              │
│ Accept Incoming Change             │  
│ Accept Both Changes                │
│ Compare Changes                    │
└────────────────────────────────────┘
```

### 3. 命令行技巧

```bash
# 查看冲突的详细信息
git diff

# 撤销合并（如果还未提交）
git merge --abort

# 查看合并前的状态
git log --merge --oneline
```

## 🔄 解决其他类型冲突

### 删除/修改冲突

```bash
# 当出现此类冲突时
CONFLICT (modify/delete): file.txt deleted in HEAD and modified in feature/branch

# 选择保留文件
git add file.txt

# 选择删除文件  
git rm file.txt
```

### 重命名冲突

```bash
# 两个分支都重命名了文件
CONFLICT (rename/rename): 
  old.txt -> new1.txt in HEAD
  old.txt -> new2.txt in feature/branch

# 手动选择最终的文件名
mv new1.txt final-name.txt
git rm new2.txt
git add final-name.txt
```

## 🎯 预防冲突的最佳实践

### 1. 频繁同步主分支

```bash
# 定期将主分支的更改合并到功能分支
git checkout feature/my-feature
git fetch origin
git merge origin/main

# 或使用 rebase 保持线性历史
git rebase origin/main
```

### 2. 保持功能分支小而专注

```markdown
✅ 好的实践：
- 一个功能分支只做一件事
- 及时完成和合并功能分支
- 避免长期运行的分支

❌ 避免的做法：
- 在一个分支上开发多个不相关功能
- 功能分支存在时间过长
- 忽略主分支的更新
```

### 3. 团队协调

```markdown
# 高风险文件的协调
- 识别经常变更的核心文件
- 团队内部协调修改时间
- 使用功能开关减少直接冲突
- 重构时提前沟通
```

### 4. 代码组织策略

```javascript
// ✅ 模块化设计减少冲突
// user/validation.js
export function validateEmail(email) { /* ... */ }

// user/formatting.js  
export function formatUserName(name) { /* ... */ }

// ❌ 所有功能混在一个大文件中
// user.js (1000+ 行，经常冲突)
```

## 📊 冲突解决流程图

```
遇到合并冲突
      ↓
   git status
   (查看冲突文件)
      ↓
   打开冲突文件
   (理解冲突内容)
      ↓
   手动编辑文件
   (选择最佳解决方案)
      ↓
   git add <文件>
   (标记冲突已解决)
      ↓
   git commit
   (完成合并)
```

## ⚠️ 常见错误和解决方案

### 错误1：直接删除冲突标记
```bash
# ❌ 只删除标记，不解决实际冲突
# 这会导致代码语法错误

# ✅ 仔细分析并选择正确的代码
```

### 错误2：忘记添加解决后的文件
```bash
# 症状：git commit 时提示还有未解决的冲突
# 解决：确保 git add 所有修改的文件
git add .
git commit
```

### 错误3：强制推送覆盖他人工作
```bash
# ❌ 危险操作
git push --force

# ✅ 安全做法
git push --force-with-lease
# 或者重新解决冲突
```

## 🎯 实践练习

### 练习1：创建并解决冲突

```bash
# 1. 创建测试仓库
git init conflict-practice
cd conflict-practice
echo "Hello World" > test.txt
git add test.txt
git commit -m "initial commit"

# 2. 创建并切换到新分支
git checkout -b feature1
echo "Hello from feature1" > test.txt
git add test.txt
git commit -m "modify in feature1"

# 3. 回到主分支并做不同的修改
git checkout main
echo "Hello from main" > test.txt  
git add test.txt
git commit -m "modify in main"

# 4. 尝试合并，观察冲突
git merge feature1

# 5. 解决冲突并完成合并
```

### 练习2：使用工具解决冲突

```bash
# 配置 VS Code 作为合并工具
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# 创建冲突并使用工具解决
git mergetool
```

## ✅ 检查清单

完成本节后，您应该能够：
- [ ] 理解冲突产生的原因
- [ ] 识别不同类型的合并冲突
- [ ] 读懂 Git 的冲突标记
- [ ] 手动解决内容冲突
- [ ] 使用工具辅助解决冲突
- [ ] 采用最佳实践预防冲突
- [ ] 处理删除/修改等特殊冲突

---

**下一步**：进行 [实践练习](./06-practice.md) 