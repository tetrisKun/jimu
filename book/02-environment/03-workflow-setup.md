# 2.3 开发环境与工作流构建

AI测试驱动开发(AITDD)要有效实践,需要构建适当的开发环境和工作流。不仅要准备工具,还需要系统化整个开发流程,以实现一致性高质量的开发。

## AITDD导入的实际经历与方法的演进

### 导入时间线

#### 2025年初开始的正式推进

**契机:**
- **Claude Sonnet 3.5** 和 **DeepSeek R1的蒸馏模型** 的出现
- 确信了一定程度的实现可以通过AI来实现
- 感受到了传统手动编码的局限性

**约5-6个月的实践经验积累:**
- 从初期的试错发展到系统化方法
- 发现提示词设计的优化模式
- 从失败案例中学习并确立最佳实践

### 方法的演进过程

#### 第一阶段:实时编码(初期方法)

**特征:**
- 与AI实时对话创建代码
- 对小规模功能实现有效
- 通过即时反馈快速修正

**有效的场景:**
- 单个文件内的小规模修改
- 快速创建原型
- 为技术调查创建示例代码

**发现的局限:**
- 大规模开发容易出现结构崩溃
- 复杂依赖关系管理困难
- 难以保持质量一致性

#### 第二阶段:与TDD结合(当前方法)

**问题认识:**
- 实时编码难以应对大规模开发
- 缺乏质量保证机制
- 需要保持设计一致性的机制

**解决方案的采用:**
- **TDD(测试驱动开发)** 的结合
- 确立 **Red-Green-Refactor-Validation** 循环
- 构建系统化工作流

**当前方法的特征:**
```
演进前(实时编码):
需求 → 直接实现 → 动作确认 → 修正 → 完成

演进后(AITDD):
需求 → TODO创建 → Red → Green → Refactor → Validation → 完成
         ↑                                          ↓
         ←←←←←←← 反馈循环 ←←←←←←←←←←
```

### 实际开发工作流的构建经验

#### 项目结构的优化过程

**初期问题:**
- 文件结构各项目参差不齐
- TODO粒度不合适
- Git历史难以追踪

**改进后的结构:**
```
project-root/
├── todo.md                    # 中心任务管理
├── docs/                      # 设计文档系统化
│   ├── requirements.md        # 明确的需求定义
│   ├── architecture.md        # 架构设计
│   └── api-spec.md           # 详细的API规格
├── src/                       # 按功能明确分离
├── tests/                     # 测试代码系统化
└── scripts/                   # 自动化脚本
```

#### TODO管理的演进过程

**初期问题:**
- TODO粒度过大("系统整体实现"等)
- 依赖关系不明确
- 进度难以追踪

**当前优化的方法:**

**适当粒度的发现:**
```markdown
# 最佳粒度(30分钟~1小时)
- [x] 用户注册API的实现
- [x] 密码验证功能
- [ ] JWT认证中间件
- [ ] 登录功能的测试追加

# 应避免的粒度
❌ 系统整体的实现(太大)
❌ 变量名的更改(太小)
```

**实践性的顺序执行策略:**
1. **从TODO列表上往下顺序处理**
2. **完全完成一个项目后再进行下一个**
3. **有依赖关系的项目调整顺序**
4. **分割成30分钟~1小时可完成的单位**

### Git工作流的实践运用

#### AITDD特化的分支策略

**实际采用的策略:**
```bash
# 每个TODO项目的分支创建模式
git checkout -b feature/user-registration    # TODO: 用户注册API
git checkout -b feature/auth-middleware      # TODO: 认证中间件
git checkout -b feature/password-validation  # TODO: 密码验证
```

**AITDD循环对应提交策略:**
```bash
# Red 阶段(创建失败测试)
git add tests/user-registration.test.js
git commit -m "Red: Add failing tests for user registration"

# Green 阶段(通过测试的最小实现)
git add src/controllers/user.js
git commit -m "Green: Implement basic user registration"

# Refactor 阶段(代码改进)
git add src/controllers/user.js src/models/user.js
git commit -m "Refactor: Extract user validation logic"

# Validation 阶段(最终验证和文档化)
git add docs/api-spec.md
git commit -m "Validation: Complete user registration with docs"
```

#### 失败时的恢复策略实践

**实际运用的判断基准:**
```bash
# 模式1: 轻微修正可对应
if [ "期待的差异" == "小" ]; then
    # 调整提示词后重新执行
    echo "详细化提示词后重试"
fi

# 模式2: 需要大幅修正
if [ "期待的差异" == "大" ]; then
    git reset --hard HEAD~1  # 返回前一个状态
    echo "重新审视提示词后重新执行"
fi

# 模式3: 多次失败
if [ "失败次数" -gt 3 ]; then
    git reset --hard <last_known_good_commit>
    echo "从根本上重新审视方法"
fi
```

**实际的恢复模式例:**
```
情况: 用户认证API的实现中错误处理不当
判断: 3次修正尝试后未改善
对应: git reset --hard HEAD~4 回到Red阶段
重新执行: 用更详细的提示词重新开始
结果: 完成了符合期待的实现
```

### 从实践中获得的重要教训

#### 成功要因的分析

**1. 阶段性方法的效果:**
- 小步骤的稳定进步
- 各阶段的质量确认
- 失败时影响范围的限定

**2. 文档化的重要性:**
- 需求定义的明确化直接关系到AI输出质量
- API规格书成为测试设计的指南
- 进度的可视化有助于维持动力

**3. 提示词优化的累积效果:**
- 通过重用相同模式提高效率
- 通过分析失败案例提高精度
- 领域特化知识的积累

#### 常见问题及对策

**问题1: AI输出质量不稳定**
```
症状: 即使相同提示词,不同日子结果不同
原因: 提示词模糊性,上下文不足
对策: 添加更具体的技术约束和例示

改善例:
"创建API"
↓
"用Express.js + Mongoose创建POST /api/users API:
- 请求: {name: string, email: string}
- 验证: email格式检查,name必填
- 响应: 201返回创建的用户信息
- 错误: 400(验证), 409(重复), 500(服务器)"
```

**问题2: 大规模项目中迷失**
```
症状: 不知道当前工作位置
原因: TODO管理不完善,进度追踪缺失
对策: 明确进度显示和下一步操作的明示

改善例:
## 当前实现状况 (2025-06-21)
- [x] 用户管理功能 (完成)
- [ ] **认证功能 (实现中: JWT middleware创建阶段)**
- [ ] 权限管理功能 (未着手)

### 下一步操作
1. JWT签名验证的实现
2. 令牌更新功能的追加
3. 登出功能的实现
```

**问题3: 测试与代码不一致**
```
症状: 测试通过但实际动作与期待不同
原因: 测试设计不完善,需求理解有偏差
对策: 创建更现实的测试用例

改善例:
# 不充分的测试
test('should create user', () => {
  expect(user).toBeDefined();
});

# 改善的测试
test('should create user with valid email and return 201', async () => {
  const userData = { name: 'John', email: 'john@example.com' };
  const response = await request(app)
    .post('/api/users')
    .send(userData)
    .expect(201);

  expect(response.body.user.email).toBe(userData.email);
  expect(response.body.user.password).toBeUndefined(); // 不包含密码
});
```

## AITDD开发工作流的整体概况

### 基本开发流程
```
TODO列表创建 → 项目选择 → AITDD执行 → 审查 → 下一个项目
     ↑                                            ↓
     ←←←←←←←←←← 根据需要调整 ←←←←←←←←←←←←←←
```

### AITDD执行的详细循环
```
Red (测试创建) → Green (实现) → Refactor (改进) → Validation (验证)
      ↑                                                    ↓
      ←←←←←←←←←←←←← 反馈循环 ←←←←←←←←←←←←←←
```

## 项目结构的设计

### 推荐目录结构

```
project-root/
├── todo.md                    # 主要TODO列表
├── docs/                      # 项目文档
│   ├── requirements.md        # 需求定义
│   ├── architecture.md        # 架构设计
│   └── api-spec.md           # API规格书
├── src/                       # 源代码
│   ├── models/               # 数据模型
│   ├── controllers/          # 控制器
│   ├── services/             # 业务逻辑
│   └── utils/                # 实用工具
├── tests/                     # 测试代码
│   ├── unit/                 # 单元测试
│   ├── integration/          # 集成测试
│   └── fixtures/             # 测试数据
├── scripts/                   # 开发用脚本
└── README.md                 # 项目概要
```

### TODO列表的创建和管理

#### TODO列表的基本格式

**todo.md 的例子:**
```markdown
# 项目TODO列表

## 当前实现对象
- [ ] 用户注册功能的实现

## 已完成
- [x] 项目初期设定
- [x] 数据库连接设定

## 未着手(优先级顺序)
1. [ ] 用户认证功能
   - [ ] 密码哈希化
   - [ ] JWT令牌生成
   - [ ] 登录API

2. [ ] 用户管理功能
   - [ ] 个人资料更新API
   - [ ] 用户删除API
   - [ ] 用户列表API

3. [ ] 安全性强化
   - [ ] 速率限制实现
   - [ ] 输入值验证强化
   - [ ] CORS设定

## 今后讨论
- [ ] 性能优化
- [ ] 部署自动化
```

#### 有效TODO粒度的设定

**适当粒度的例子:**
- ✅ `用户注册API的实现`(30分钟~1小时)
- ✅ `密码验证功能`(30分钟~1小时)
- ✅ `JWT认证中间件`(30分钟~1小时)

**应避免的粒度:**
- ❌ `系统整体的实现`(太大)
- ❌ `变量名的变更`(太小)

### TODO的执行策略

#### 顺序执行方法
```markdown
执行方针:
1. 从TODO列表上往下顺序处理
2. 完全完成一个项目后再进行下一个
3. 有依赖关系的项目调整顺序
4. 分割成30分钟~1小时可完成的单位
```

#### 依赖关系的管理
```markdown
依赖关系的例子:
- 用户模型 → 用户注册API → 用户认证
- 数据库设计 → 迁移 → API实现
- 基本功能 → 错误处理 → 安全性强化
```

## Git工作流的设定

### AITDD向分支策略

#### 基本分支模型
```bash
main                    # 生产环境用
├── develop            # 开发集成用
└── feature/todo-item  # 各TODO项目用
```

#### 分支创建的实例
```bash
# 为每个TODO项目创建分支
git checkout -b feature/user-registration
git checkout -b feature/user-authentication
git checkout -b feature/password-validation

# 按功能群汇总的情况
git checkout -b feature/user-management
git checkout -b feature/security-enhancement
```

### 提交策略

#### 对应AITDD循环的提交
```bash
# Red 阶段(测试创建)
git add tests/
git commit -m "Red: Add tests for user registration"

# Green 阶段(实现)
git add src/
git commit -m "Green: Implement user registration functionality"

# Refactor 阶段(改进)
git add src/
git commit -m "Refactor: Improve user registration code structure"

# Validation 阶段(验证)
git add .
git commit -m "Validation: Complete user registration with documentation"
```

#### 失败时的恢复策略
```bash
# AI未输出预期结果的情况
git reset --hard HEAD~1  # 取消最后提交
# 或
git reset --hard <commit-hash>  # 回到特定提交

# 调整提示词后重新执行
# 成功后新提交
```

## 实践的Next Steps

### 环境构建完成后的行动指南

1. **向第3章过渡准备**
   - AITDD流程的详细理解
   - Red-Green-Refactor-Validation循环的掌握
   - 实际开发流程的体验

2. **第一个项目计划**
   - 小规模示例项目的设计
   - 明确的功能需求定义
   - 在可实现范围内创建TODO列表

3. **持续改进的准备**
   - 提示词设计技能的提高
   - 掌握与AI有效对话的模式
   - 审查和质量管理的实践

### 成功的重要要点

#### 采用阶段性方法
- **从小处开始**: 从简单功能开始
- **逐渐扩展**: 积累成功经验
- **从失败中学习**: 不畏惧git reset的试错

#### 对质量的持续关注
- **测试优先**: 一定从测试开始写
- **审查习惯化**: AI生成的代码也必须确认
- **文档化实践**: 适当记录实现内容

#### 团队实践准备
- **构建共同理解**: 与团队成员的认知统一
- **工具统一**: 在相同开发环境中工作
- **知识共享**: 共享成功・失败案例

## 环境构建的完成确认

### 最终检查清单

- [ ] **基本工具**
  - [ ] Claude Sonnet 4 (Claude Code) 可用
  - [ ] VS Code 已适当设定
  - [ ] Git仓库已初始化

- [ ] **项目结构**
  - [ ] 已创建推荐的目录结构
  - [ ] todo.md文件已准备
  - [ ] 基本配置文件已配置

- [ ] **开发工作流**
  - [ ] Git分支策略已决定
  - [ ] 提交规则已定义
  - [ ] 测试环境已确认动作

- [ ] **文档化・监控**
  - [ ] README.md已创建
  - [ ] 日志设定已完成
  - [ ] 调试环境已准备

### 动作确认测试

```bash
# 基本动作确认
npm test                     # 测试执行
npm run test:coverage        # 覆盖率确认
git status                   # Git状态确认
git log --oneline -5         # 最近的提交确认

# Claude Code的连接确认
# 用VS Code打开项目
# 确认Claude Code插件正常工作
# 向AI请求创建简单测试用例以确认动作
```

### 故障排除

#### 常见问题及解决方案

**无法连接Claude Code**
```bash
# 认证确认
# Pro计划的有效性确认
# 网络设定确认
```

**测试环境发生错误**
```bash
# 重新安装依赖关系
npm install

# 清除包缓存
npm cache clean --force
```

**Git操作错误**
```bash
# 重新设定认证信息
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## 总结

在第2章中,我们学习了构建AITDD实践所需的全面开发环境的方法。重要要点如下:

### 主要成果
1. **工具设置**: 以Claude Sonnet 4为中心构建开发环境
2. **工作流设计**: 从TODO管理到Git流程的系统化流程
3. **质量管理基础**: 测试环境、日志、调试功能的完善

### 准备下一章
环境构建完成后,接下来将学习AITDD的实际流程。在第3章"AITDD流程详解"中,将掌握Red-Green-Refactor-Validation循环的具体实践方法。

**学习要点:**
- 各阶段的具体工作内容
- 与AI有效对话的方法
- 质量管理和审查的技巧

环境已就绪,现在已做好与AI协作进行软件开发的准备。在下一章中,让我们体验AITDD的真正价值。
