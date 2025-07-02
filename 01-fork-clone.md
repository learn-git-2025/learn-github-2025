# 1. Fork 和 Clone 基础

Fork 和 Clone 是 GitHub 协作的起点。通过本节学习，您将了解如何参与其他人的项目开发。

## 🎯 本节目标

- 理解 Fork 和 Clone 的概念和区别
- 学会 Fork 项目到自己的账户
- 掌握 Clone 仓库到本地的方法
- 建立本地开发环境

## 💡 核心概念

### Fork 是什么？

**Fork** 是在 GitHub 上创建项目副本的操作，它会将原项目完整复制到您的 GitHub 账户下。

```
原项目：github.com/original-owner/awesome-project
Fork后：github.com/your-username/awesome-project
```

### Clone 是什么？

**Clone** 是将 GitHub 上的仓库下载到本地计算机的操作。

### Fork vs Clone

| 操作 | 位置 | 目的 | 权限 |
|------|------|------|------|
| Fork | GitHub 服务器 | 创建可编辑的项目副本 | 完全控制 Fork 的仓库 |
| Clone | 本地计算机 | 在本地开发 | 根据原仓库权限决定 |

## 🛠️ 实操步骤

### 第一步：Fork 项目

1. **找到想要贡献的项目**
   - 浏览 GitHub 或通过搜索找到目标项目
   - 例如：`https://github.com/original-owner/awesome-project`

2. **执行 Fork 操作**
   ```
   在项目页面右上角点击 "Fork" 按钮
   ↓
   选择 Fork 到您的账户
   ↓
   等待 Fork 完成
   ```

3. **验证 Fork 成功**
   - 检查您的 GitHub 账户下是否出现了新仓库
   - URL 应该是：`https://github.com/your-username/awesome-project`

### 第二步：Clone 到本地

1. **获取 Clone 地址**
   ```bash
   # 在您 Fork 的仓库页面，点击绿色的 "Code" 按钮
   # 复制 HTTPS 或 SSH 地址
   ```

2. **执行 Clone 命令**
   ```bash
   # 使用 HTTPS（推荐初学者）
   git clone https://github.com/your-username/awesome-project.git
   
   # 或使用 SSH（需要配置 SSH 密钥）
   git clone git@github.com:your-username/awesome-project.git
   ```

3. **进入项目目录**
   ```bash
   cd awesome-project
   ls -la  # 查看项目文件
   ```

### 第三步：配置远程仓库

为了保持与原项目同步，需要添加原项目作为上游仓库：

```bash
# 添加原项目为 upstream
git remote add upstream https://github.com/original-owner/awesome-project.git

# 验证远程仓库配置
git remote -v
```

预期输出：
```
origin    https://github.com/your-username/awesome-project.git (fetch)
origin    https://github.com/your-username/awesome-project.git (push)
upstream  https://github.com/original-owner/awesome-project.git (fetch)
upstream  https://github.com/original-owner/awesome-project.git (push)
```

## 📊 工作流程图

```
[原项目] ──Fork──> [您的Fork] ──Clone──> [本地仓库]
    ↑                   ↑                    ↓
    └── upstream ────── origin ──── 推送更改 ──┘
```

## ⚠️ 常见问题

### Q: Fork 和直接 Clone 有什么区别？
**A:** 直接 Clone 别人的项目，您无法推送更改。Fork 后 Clone，您可以自由修改并推送到自己的 Fork。

### Q: 如何保持 Fork 与原项目同步？
**A:** 使用以下命令：
```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Q: 可以 Fork 私有仓库吗？
**A:** 只有当您有读取权限时才能 Fork 私有仓库。

## 🎯 实践练习

1. **找一个感兴趣的开源项目并 Fork 它**
2. **Clone 到本地**
3. **配置 upstream 远程仓库**
4. **创建一个简单的修改并推送到您的 Fork**

## ✅ 检查清单

完成本节后，您应该能够：
- [ ] 解释 Fork 和 Clone 的区别
- [ ] 成功 Fork 一个 GitHub 项目
- [ ] Clone 仓库到本地
- [ ] 配置 origin 和 upstream 远程仓库
- [ ] 理解基本的协作工作流程

---

**下一步**：学习 [分支协作策略](./02-branch-strategy.md) 