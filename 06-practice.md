# 6. 实践练习

通过真实场景的练习，巩固您在前面章节学到的协作技能。本节提供了从基础到高级的实践项目。

## 🎯 本节目标

- 在模拟环境中应用协作技能
- 体验完整的团队协作流程
- 处理真实项目中的常见情况
- 建立协作的肌肉记忆

## 🚀 练习环境准备

### 创建练习仓库

```bash
# 1. 创建本地练习项目
mkdir collaboration-practice
cd collaboration-practice
git init

# 2. 创建基础项目结构
mkdir -p src tests docs
echo "# 团队协作练习项目" > README.md
echo "console.log('Hello, Team!');" > src/app.js
echo "// 测试文件" > tests/app.test.js

# 3. 初始提交
git add .
git commit -m "feat: initialize project structure"

# 4. 创建 GitHub 仓库并推送
# (在 GitHub 上创建仓库后)
git remote add origin https://github.com/yourusername/collaboration-practice.git
git push -u origin main
```

## 📚 练习1：基础协作流程

### 场景
您加入了一个项目，需要添加一个用户登录功能。

### 任务步骤

#### 第一步：Fork 和 Clone
```bash
# 1. 在 GitHub 上 Fork 项目
# 2. Clone 您的 Fork
git clone https://github.com/yourusername/collaboration-practice.git
cd collaboration-practice

# 3. 配置 upstream
git remote add upstream https://github.com/original/collaboration-practice.git
git remote -v
```

#### 第二步：创建功能分支
```bash
# 1. 确保在最新的主分支
git checkout main
git pull upstream main

# 2. 创建功能分支
git checkout -b feature/user-login
```

#### 第三步：开发功能
```bash
# 1. 创建登录模块
cat > src/auth.js << 'EOF'
/**
 * 用户认证模块
 */
class UserAuth {
  constructor() {
    this.users = [];
  }
  
  /**
   * 用户注册
   * @param {string} username 用户名
   * @param {string} password 密码
   * @returns {boolean} 注册是否成功
   */
  register(username, password) {
    if (!username || !password) {
      throw new Error('用户名和密码不能为空');
    }
    
    if (this.users.find(u => u.username === username)) {
      throw new Error('用户名已存在');
    }
    
    this.users.push({ username, password });
    return true;
  }
  
  /**
   * 用户登录
   * @param {string} username 用户名
   * @param {string} password 密码
   * @returns {boolean} 登录是否成功
   */
  login(username, password) {
    const user = this.users.find(u => 
      u.username === username && u.password === password
    );
    return !!user;
  }
}

module.exports = UserAuth;
EOF

# 2. 创建测试文件
cat > tests/auth.test.js << 'EOF'
const UserAuth = require('../src/auth');

describe('UserAuth', () => {
  let auth;
  
  beforeEach(() => {
    auth = new UserAuth();
  });
  
  test('应该能够注册新用户', () => {
    expect(auth.register('testuser', 'password123')).toBe(true);
  });
  
  test('应该拒绝重复的用户名', () => {
    auth.register('testuser', 'password123');
    expect(() => {
      auth.register('testuser', 'different');
    }).toThrow('用户名已存在');
  });
  
  test('应该能够登录已注册用户', () => {
    auth.register('testuser', 'password123');
    expect(auth.login('testuser', 'password123')).toBe(true);
  });
  
  test('应该拒绝错误的密码', () => {
    auth.register('testuser', 'password123');
    expect(auth.login('testuser', 'wrongpassword')).toBe(false);
  });
});
EOF

# 3. 更新主应用文件
cat > src/app.js << 'EOF'
const UserAuth = require('./auth');

console.log('🚀 团队协作练习项目启动');

// 演示用户认证功能
const auth = new UserAuth();

try {
  // 注册用户
  auth.register('alice', 'password123');
  auth.register('bob', 'securepass');
  
  console.log('✅ 用户注册成功');
  
  // 测试登录
  if (auth.login('alice', 'password123')) {
    console.log('✅ Alice 登录成功');
  }
  
  if (!auth.login('alice', 'wrongpass')) {
    console.log('❌ 错误密码被正确拒绝');
  }
  
} catch (error) {
  console.error('❌ 错误:', error.message);
}
EOF
```

#### 第四步：提交和推送
```bash
# 1. 检查更改
git status
git diff

# 2. 添加文件
git add .

# 3. 提交更改
git commit -m "feat: add user authentication system

- Add UserAuth class with register/login methods
- Include comprehensive error handling
- Add unit tests for all authentication scenarios
- Update main app to demonstrate functionality

Closes #1"

# 4. 推送分支
git push -u origin feature/user-login
```

#### 第五步：创建 Pull Request
```markdown
在 GitHub 上创建 PR，使用以下模板：

## 🎯 变更内容
添加用户认证系统，支持用户注册和登录功能。

## 🛠️ 实现方案
- 创建 `UserAuth` 类封装认证逻辑
- 实现用户注册和登录方法
- 添加错误处理和输入验证
- 包含完整的单元测试

## 🧪 测试
- [x] 用户注册功能测试
- [x] 用户登录功能测试
- [x] 错误处理测试
- [x] 边界条件测试

## 📋 检查清单
- [x] 代码遵循项目规范
- [x] 添加了详细注释
- [x] 包含完整的测试用例
- [x] 更新了示例代码
```

## 📚 练习2：处理代码审查

### 场景
您的 PR 收到了审查反馈，需要根据建议进行改进。

### 模拟审查反馈

```markdown
@审查者评论：
1. 🔴 安全问题：密码应该加密存储，不能明文保存
2. 🟡 代码质量：建议添加密码强度验证
3. 🔵 优化建议：考虑使用更安全的哈希算法
4. 💭 讨论：是否需要支持用户角色系统？
```

### 响应反馈

```bash
# 1. 在当前分支上继续开发
git checkout feature/user-login

# 2. 安装必要的依赖（模拟）
# npm install bcrypt

# 3. 改进代码
cat > src/auth.js << 'EOF'
/**
 * 用户认证模块 - 改进版
 */
const crypto = require('crypto');

class UserAuth {
  constructor() {
    this.users = [];
  }
  
  /**
   * 密码强度验证
   * @param {string} password 密码
   * @returns {boolean} 密码是否符合要求
   */
  validatePasswordStrength(password) {
    // 至少8位，包含大小写字母和数字
    const strongPassword = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/;
    return strongPassword.test(password);
  }
  
  /**
   * 密码哈希
   * @param {string} password 明文密码
   * @returns {string} 哈希后的密码
   */
  hashPassword(password) {
    return crypto.createHash('sha256').update(password).digest('hex');
  }
  
  /**
   * 用户注册
   * @param {string} username 用户名
   * @param {string} password 密码
   * @returns {boolean} 注册是否成功
   */
  register(username, password) {
    if (!username || !password) {
      throw new Error('用户名和密码不能为空');
    }
    
    if (!this.validatePasswordStrength(password)) {
      throw new Error('密码强度不足：需要至少8位，包含大小写字母和数字');
    }
    
    if (this.users.find(u => u.username === username)) {
      throw new Error('用户名已存在');
    }
    
    const hashedPassword = this.hashPassword(password);
    this.users.push({ username, password: hashedPassword });
    return true;
  }
  
  /**
   * 用户登录
   * @param {string} username 用户名
   * @param {string} password 密码
   * @returns {boolean} 登录是否成功
   */
  login(username, password) {
    const hashedPassword = this.hashPassword(password);
    const user = this.users.find(u => 
      u.username === username && u.password === hashedPassword
    );
    return !!user;
  }
}

module.exports = UserAuth;
EOF

# 4. 更新测试
cat > tests/auth.test.js << 'EOF'
const UserAuth = require('../src/auth');

describe('UserAuth', () => {
  let auth;
  
  beforeEach(() => {
    auth = new UserAuth();
  });
  
  test('应该验证密码强度', () => {
    expect(auth.validatePasswordStrength('weak')).toBe(false);
    expect(auth.validatePasswordStrength('StrongPass123')).toBe(true);
  });
  
  test('应该拒绝弱密码注册', () => {
    expect(() => {
      auth.register('testuser', 'weak');
    }).toThrow('密码强度不足');
  });
  
  test('应该能够注册强密码用户', () => {
    expect(auth.register('testuser', 'StrongPass123')).toBe(true);
  });
  
  test('密码应该被加密存储', () => {
    auth.register('testuser', 'StrongPass123');
    const user = auth.users.find(u => u.username === 'testuser');
    expect(user.password).not.toBe('StrongPass123');
    expect(user.password.length).toBe(64); // SHA256 输出长度
  });
  
  test('应该能够登录已注册用户', () => {
    auth.register('testuser', 'StrongPass123');
    expect(auth.login('testuser', 'StrongPass123')).toBe(true);
  });
});
EOF

# 5. 提交改进
git add .
git commit -m "fix: improve security and password validation

- Add password strength validation (min 8 chars, mixed case, numbers)
- Implement password hashing using SHA256
- Update tests to verify security improvements
- Addresses security concerns from code review

Co-authored-by: Reviewer <reviewer@example.com>"

# 6. 推送更新
git push
```

### 回复审查评论

```markdown
@your-username 回复:
感谢详细的审查反馈！我已经按照建议进行了改进：

1. ✅ **安全问题已修复**: 现在使用SHA256对密码进行哈希处理，不再明文存储
2. ✅ **添加密码强度验证**: 要求至少8位，包含大小写字母和数字
3. ✅ **改进测试覆盖**: 新增了密码强度和加密相关的测试用例

关于用户角色系统的建议很好，我觉得这可以作为后续的功能增强。是否考虑为此创建一个新的issue来跟踪？

请再次审查，谢谢！
```

## 📚 练习3：解决合并冲突

### 场景
在您开发功能的同时，主分支有了新的更新，导致合并冲突。

### 创建冲突场景

```bash
# 1. 模拟主分支更新（在另一个本地分支）
git checkout main
git checkout -b simulate-main-updates

# 2. 修改相同文件
cat > src/app.js << 'EOF'
const UserAuth = require('./auth');

console.log('🚀 团队协作练习项目 v2.0 启动');

// 新增配置系统
const config = {
  appName: '协作练习',
  version: '2.0.0',
  environment: 'development'
};

console.log(`📱 应用: ${config.appName} v${config.version}`);

// 演示用户认证功能
const auth = new UserAuth();
console.log('🔐 认证系统已加载');
EOF

# 3. 提交更新
git add .
git commit -m "feat: add application configuration system"

# 4. 回到功能分支
git checkout feature/user-login

# 5. 尝试合并主分支更新
git merge simulate-main-updates
```

### 解决冲突

```bash
# 1. 查看冲突状态
git status

# 2. 编辑冲突文件，合并两个版本的改进
cat > src/app.js << 'EOF'
const UserAuth = require('./auth');

console.log('🚀 团队协作练习项目 v2.0 启动');

// 新增配置系统
const config = {
  appName: '协作练习',
  version: '2.0.0',
  environment: 'development'
};

console.log(`📱 应用: ${config.appName} v${config.version}`);

// 演示用户认证功能
const auth = new UserAuth();
console.log('🔐 认证系统已加载');

try {
  // 注册用户 - 使用强密码
  auth.register('alice', 'StrongPass123');
  auth.register('bob', 'SecurePass456');
  
  console.log('✅ 用户注册成功');
  
  // 测试登录
  if (auth.login('alice', 'StrongPass123')) {
    console.log('✅ Alice 登录成功');
  }
  
  if (!auth.login('alice', 'wrongpass')) {
    console.log('❌ 错误密码被正确拒绝');
  }
  
} catch (error) {
  console.error('❌ 错误:', error.message);
}
EOF

# 3. 标记冲突已解决
git add src/app.js

# 4. 完成合并
git commit -m "resolve merge conflict in app.js

Merged application configuration system with authentication demo.
Both features are now working together properly."
```

## 📚 练习4：团队协作模拟

### 场景
模拟多人协作开发同一项目的情况。

### 设置团队成员角色

```bash
# 成员A：添加数据验证功能
git checkout main
git checkout -b feature/data-validation

# 创建验证模块
cat > src/validator.js << 'EOF'
/**
 * 数据验证工具
 */
class DataValidator {
  /**
   * 验证邮箱格式
   */
  static validateEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
  }
  
  /**
   * 验证手机号
   */
  static validatePhone(phone) {
    const phoneRegex = /^1[3-9]\d{9}$/;
    return phoneRegex.test(phone);
  }
}

module.exports = DataValidator;
EOF

git add .
git commit -m "feat: add data validation utilities"
git push -u origin feature/data-validation

# 成员B：添加日志功能
git checkout main
git checkout -b feature/logging-system

# 创建日志模块
cat > src/logger.js << 'EOF'
/**
 * 简单日志系统
 */
class Logger {
  static log(level, message) {
    const timestamp = new Date().toISOString();
    console.log(`[${timestamp}] ${level.toUpperCase()}: ${message}`);
  }
  
  static info(message) {
    this.log('info', message);
  }
  
  static warn(message) {
    this.log('warn', message);
  }
  
  static error(message) {
    this.log('error', message);
  }
}

module.exports = Logger;
EOF

git add .
git commit -m "feat: add logging system"
git push -u origin feature/logging-system
```

### 协调合并

```bash
# 作为项目维护者，按顺序合并功能
git checkout main

# 合并第一个功能
git merge feature/data-validation
git push origin main

# 合并第二个功能
git merge feature/logging-system
git push origin main

# 清理已合并的分支
git branch -d feature/data-validation
git branch -d feature/logging-system
git push origin --delete feature/data-validation
git push origin --delete feature/logging-system
```

## 🎯 高级练习：开源贡献

### 找一个真实的开源项目

```bash
# 1. 选择项目（建议选择标记为 "good first issue" 的）
# 例如：https://github.com/microsoft/vscode
# 或者：https://github.com/facebook/react

# 2. Fork 项目
# 3. 找到适合的 issue
# 4. 按照项目的贡献指南进行开发
# 5. 提交 PR 并参与讨论
```

### 贡献检查清单

```markdown
开源贡献前检查：
- [ ] 阅读项目的 CONTRIBUTING.md
- [ ] 查看代码风格指南
- [ ] 理解项目的开发流程
- [ ] 在 issue 中表达贡献意图
- [ ] 小步快跑，从简单问题开始
- [ ] 尊重维护者的时间和决定
```

## 📊 练习成果评估

### 自我评估清单

完成练习后，检查您是否能够：

#### 基础技能
- [ ] 熟练使用 Fork-Clone 工作流
- [ ] 创建规范的功能分支
- [ ] 编写清晰的提交信息
- [ ] 创建有效的 Pull Request

#### 协作技能
- [ ] 响应代码审查反馈
- [ ] 进行建设性的代码审查
- [ ] 有效解决合并冲突
- [ ] 与团队成员沟通协调

#### 高级技能
- [ ] 处理复杂的合并场景
- [ ] 维护干净的提交历史
- [ ] 参与开源项目贡献
- [ ] 指导新团队成员

## 🎉 总结

通过这些实践练习，您已经体验了完整的团队协作流程。继续在真实项目中应用这些技能，并随着经验的积累不断改进您的协作方式。

记住，优秀的协作不仅仅是技术技能，更重要的是沟通、耐心和对团队的贡献精神。

---

**恭喜！您已完成团队协作教程的所有内容。** 🚀 