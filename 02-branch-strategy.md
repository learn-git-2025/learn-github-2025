# 2. 分支协作策略

在团队协作中，合理的分支策略是保证代码质量和开发效率的基础。本节将介绍最常用的功能分支工作流。

## 🎯 本节目标

- 理解功能分支工作流的概念
- 掌握分支命名规范
- 学会创建、切换和管理分支
- 了解分支合并的最佳实践

## 💡 功能分支工作流

### 核心理念

功能分支工作流的基本思想是：**每个新功能都在专门的分支上开发，开发完成后通过 Pull Request 合并到主分支**。

### 分支类型

```
main/master     ← 生产环境代码，始终保持稳定
├── feature/xxx ← 功能开发分支
├── bugfix/xxx  ← 错误修复分支
├── hotfix/xxx  ← 紧急修复分支
└── release/xxx ← 发布准备分支
```

## 🏷️ 分支命名规范

良好的命名规范能让团队成员快速理解分支用途：

### 功能分支
```bash
feature/user-authentication    # 用户认证功能
feature/shopping-cart         # 购物车功能
feature/email-notifications   # 邮件通知功能
```

### 修复分支
```bash
bugfix/login-error           # 登录错误修复
bugfix/payment-timeout       # 支付超时修复
hotfix/security-vulnerability # 安全漏洞紧急修复
```

### 其他分支
```bash
release/v1.2.0              # 版本发布分支
experiment/new-ui           # 实验性功能
refactor/database-layer     # 重构分支
```

## 🛠️ 分支操作实践

### 创建功能分支

```bash
# 1. 确保在主分支上
git checkout main

# 2. 拉取最新代码
git pull origin main

# 3. 创建并切换到新分支
git checkout -b feature/user-profile

# 或者分步操作
git branch feature/user-profile    # 创建分支
git checkout feature/user-profile  # 切换分支
```

### 开发和提交

```bash
# 在功能分支上进行开发
echo "新功能代码" > user-profile.js

# 添加和提交更改
git add .
git commit -m "feat: add user profile page

- Add user profile component
- Implement profile editing
- Add validation for profile fields"
```

### 推送分支

```bash
# 首次推送分支
git push -u origin feature/user-profile

# 后续推送
git push
```

### 保持分支同步

```bash
# 定期同步主分支的最新更改
git checkout main
git pull origin main

git checkout feature/user-profile
git merge main
# 或使用 rebase（保持线性历史）
git rebase main
```

## 📋 工作流程示例

### 完整的功能开发流程

```bash
# 1. 准备工作
git checkout main
git pull origin main

# 2. 创建功能分支
git checkout -b feature/search-function

# 3. 开发功能
# ... 编写代码 ...
git add .
git commit -m "feat: implement basic search"

# 4. 继续开发
# ... 更多代码 ...
git add .
git commit -m "feat: add search filters"

# 5. 推送分支
git push -u origin feature/search-function

# 6. 在 GitHub 上创建 Pull Request
# （下一节详细介绍）

# 7. 分支合并后清理
git checkout main
git pull origin main
git branch -d feature/search-function
```

## 🎨 提交信息规范

使用语义化提交信息：

```bash
feat: 新功能
fix: 错误修复
docs: 文档更新
style: 代码格式（不影响功能）
refactor: 重构
test: 测试相关
chore: 构建过程或辅助工具的变动

# 示例
git commit -m "feat: add user login functionality"
git commit -m "fix: resolve payment gateway timeout issue"
git commit -m "docs: update API documentation"
```

## 📊 分支策略对比

| 策略 | 优点 | 缺点 | 适用场景 |
|------|------|------|----------|
| 功能分支 | 功能隔离，代码审查 | 分支较多 | 大多数项目 |
| GitFlow | 完整流程，适合发布 | 复杂，分支多 | 传统软件发布 |
| GitHub Flow | 简单，持续部署 | 需要完善的CI/CD | Web应用，敏捷开发 |

## ⚠️ 常见陷阱和解决方案

### 1. 分支过时问题
**问题**：功能分支开发时间过长，与主分支差异过大
**解决**：定期 merge 或 rebase 主分支

### 2. 分支命名混乱
**问题**：分支名称不规范，难以理解
**解决**：制定并遵守团队命名规范

### 3. 分支未及时清理
**问题**：已合并的分支没有删除，仓库混乱
**解决**：合并后及时删除本地和远程分支

## 🎯 实践练习

1. **创建一个功能分支**
   ```bash
   git checkout -b feature/add-navigation
   ```

2. **进行一些修改并提交**
   ```bash
   # 创建文件或修改代码
   git add .
   git commit -m "feat: add navigation component"
   ```

3. **推送分支**
   ```bash
   git push -u origin feature/add-navigation
   ```

4. **模拟同步主分支**
   ```bash
   git checkout main
   git pull origin main
   git checkout feature/add-navigation
   git merge main
   ```

## ✅ 检查清单

完成本节后，您应该能够：
- [ ] 理解功能分支工作流的优势
- [ ] 遵循分支命名规范
- [ ] 创建和管理功能分支
- [ ] 保持分支与主分支同步
- [ ] 编写规范的提交信息
- [ ] 合并后清理分支

---

**下一步**：学习 [Pull Request 流程](./03-pull-request.md) 