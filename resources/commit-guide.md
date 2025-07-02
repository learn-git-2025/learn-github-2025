# Git 提交信息指南

## 📝 提交信息格式

### 基本结构
```
<type>(<scope>): <subject>

<body>

<footer>
```

### 类型 (type)
| 类型 | 描述 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat: add user login system` |
| `fix` | 错误修复 | `fix: resolve payment timeout issue` |
| `docs` | 文档更新 | `docs: update API documentation` |
| `style` | 代码格式（不影响功能） | `style: fix indentation in user.js` |
| `refactor` | 重构（不是新功能也不是修复bug） | `refactor: simplify validation logic` |
| `test` | 添加或修改测试 | `test: add unit tests for auth module` |
| `chore` | 构建过程或辅助工具的变动 | `chore: update dependencies` |
| `perf` | 性能优化 | `perf: improve database query speed` |
| `ci` | CI/CD 相关 | `ci: add automated testing workflow` |
| `build` | 构建系统相关 | `build: configure webpack for production` |

### 范围 (scope) - 可选
指定提交影响的模块或组件：
```
feat(auth): add password reset functionality
fix(payment): handle timeout errors properly
docs(api): update endpoint documentation
```

### 主题 (subject)
- 使用祈使句：`add` 而不是 `added` 或 `adds`
- 不要大写首字母
- 不要以句号结尾
- 限制在 50 个字符以内

### 正文 (body) - 可选
- 用空行分隔主题和正文
- 解释**为什么**而不是**什么**
- 每行限制在 72 个字符以内

### 页脚 (footer) - 可选
- 引用相关 Issue：`Closes #123`, `Fixes #456`
- 标记破坏性变更：`BREAKING CHANGE:`

## ✅ 好的示例

### 简单提交
```
feat: add user authentication

Implement basic login/logout functionality with JWT tokens.
Includes password hashing and session management.

Closes #42
```

### 修复错误
```
fix: prevent memory leak in image processing

The previous implementation didn't properly dispose of canvas
elements, causing memory usage to grow over time.

Fixes #67
```

### 破坏性变更
```
feat: change API response format

BREAKING CHANGE: API responses now return data in 'result' field
instead of directly returning the data object.

Migration guide: wrap existing response handlers with result.data
```

## ❌ 避免的做法

### 模糊的信息
```
❌ fix bug
❌ update files
❌ changes
❌ wip
```

### 过长的主题
```
❌ feat: add user authentication system with password validation and email verification
```

### 无意义的信息
```
❌ feat: add some stuff
❌ fix: fix the thing that was broken
```

## 🛠️ 实用技巧

### 1. 使用 Git 钩子
创建 `.gitmessage` 模板：
```bash
git config commit.template .gitmessage
```

### 2. 原子提交
每个提交只做一件事：
```bash
# ✅ 好的做法
git commit -m "feat: add user validation"
git commit -m "test: add validation unit tests"

# ❌ 避免
git commit -m "feat: add user validation and tests and fix bug"
```

### 3. 提交前检查
```bash
# 查看将要提交的内容
git diff --staged

# 检查提交信息
git log --oneline -5
```

## 📊 团队约定示例

### 项目特定的类型
```
ui: 用户界面相关变更
api: API 接口变更
db: 数据库相关变更
config: 配置文件变更
```

### 范围约定
```
feat(frontend): 前端功能
feat(backend): 后端功能
feat(mobile): 移动端功能
```

## 🔧 工具推荐

### Commitizen
```bash
npm install -g commitizen
npm install -g cz-conventional-changelog
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

使用：
```bash
git cz  # 代替 git commit
```

### Conventional Commits 规范
遵循 [Conventional Commits](https://www.conventionalcommits.org/) 标准，便于自动化工具处理。

---

记住：**好的提交信息是给未来的自己和同事的礼物！** 