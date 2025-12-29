---
description: 作为项目初始设置选定技术栈。如果已有 CLAUDE.md 则可以省略
---

# init-tech-stack

## 目的

通过与用户的交互式对话,创建适合项目的技术栈定义,并生成 `docs/tech-stack.md` 文件。

## 前提条件

- 在项目根目录下执行
- `docs/` 目录存在(如果不存在则创建)

## 执行流程

此命令使用 **AskUserQuestion 工具**进行分阶段访谈。

### Phase 1: 收集项目基本信息

首先,使用 AskUserQuestion 工具同时提出以下 3 个问题:

```
AskUserQuestion({
  questions: [
    {
      question: "请告诉我项目的类型",
      header: "项目类型",
      multiSelect: false,
      options: [
        { label: "Web应用程序", description: "在浏览器中运行的应用程序" },
        { label: "移动应用", description: "面向智能手机/平板电脑的应用" },
        { label: "API/后端", description: "主要提供API的服务" },
        { label: "桌面应用", description: "面向PC的本地应用" },
        { label: "库/SDK", description: "面向其他开发者的工具" },
        { label: "全栈", description: "前端和后端都有" }
      ]
    },
    {
      question: "请告诉我开发团队的规模",
      header: "团队规模",
      multiSelect: false,
      options: [
        { label: "个人开发", description: "1人开发" },
        { label: "小型团队", description: "2-5人" },
        { label: "中型团队", description: "6-15人" },
        { label: "大型团队", description: "16人以上" }
      ]
    },
    {
      question: "预计的开发周期是多久?",
      header: "开发周期",
      multiSelect: false,
      options: [
        { label: "原型/MVP", description: "1-2个月" },
        { label: "短期项目", description: "3-6个月" },
        { label: "中期项目", description: "6个月-1年" },
        { label: "长期项目", description: "1年以上" }
      ]
    }
  ]
})
```

### Phase 2: 收集技术约束・需求

接下来,使用 AskUserQuestion 工具同时提出以下 3 个问题:

```
AskUserQuestion({
  questions: [
    {
      question: "是否需要与现有系统或数据库集成?",
      header: "现有集成",
      multiSelect: false,
      options: [
        { label: "新建构筑", description: "全部新建" },
        { label: "现有DB集成", description: "使用现有数据库" },
        { label: "现有API集成", description: "与现有API集成" },
        { label: "遗留系统迁移", description: "从现有系统迁移" }
      ]
    },
    {
      question: "请告诉我预期的负载和性能需求",
      header: "性能",
      multiSelect: false,
      options: [
        { label: "轻负载", description: "同时用户数10人以下,响应3秒以内" },
        { label: "中负载", description: "同时用户数100人左右,响应1秒以内" },
        { label: "高负载", description: "同时用户数1000人以上,响应0.5秒以内" },
        { label: "未定/不明", description: "尚未确定详细信息" }
      ]
    },
    {
      question: "请告诉我安全的重要程度",
      header: "安全性",
      multiSelect: false,
      options: [
        { label: "基本级别", description: "一般的Web安全对策" },
        { label: "高级", description: "处理个人信息、金融等" },
        { label: "企业级", description: "面向企业,有合规要求" },
        { label: "未定/不明", description: "尚未确定详细信息" }
      ]
    }
  ]
})
```

### Phase 3: 收集团队・技能状况

接下来,使用 AskUserQuestion 工具同时提出以下 2 个问题:

```
AskUserQuestion({
  questions: [
    {
      question: "请告诉我团队成员的技术经验(可多选)",
      header: "技术技能",
      multiSelect: true,
      options: [
        { label: "JavaScript/TypeScript", description: "经验丰富" },
        { label: "Python", description: "经验丰富" },
        { label: "Java/Kotlin", description: "经验丰富" },
        { label: "C#/.NET", description: "经验丰富" },
        { label: "Go/Rust", description: "经验丰富" },
        { label: "React/Vue/Angular", description: "经验丰富" },
        { label: "数据库设计", description: "经验丰富" },
        { label: "云(AWS/Azure/GCP)", description: "经验丰富" },
        { label: "Docker/Kubernetes", description: "经验丰富" },
        { label: "技术技能有限", description: "希望边学习边推进" }
      ]
    },
    {
      question: "关于学习新技术,您怎么考虑?",
      header: "学习成本",
      multiSelect: false,
      options: [
        { label: "积极采用新技术", description: "想用最新技术挑战" },
        { label: "注重平衡", description: "平衡新技术和稳定技术" },
        { label: "优先稳定技术", description: "用成熟技术可靠开发" },
        { label: "活用现有技能", description: "最大限度活用团队知识" }
      ]
    }
  ]
})
```

### Phase 4: 收集运维・基础设施需求

最后,使用 AskUserQuestion 工具同时提出以下 2 个问题:

```
AskUserQuestion({
  questions: [
    {
      question: "您计划在哪里运行应用程序?",
      header: "托管",
      multiSelect: false,
      options: [
        { label: "云", description: "AWS/Azure/GCP" },
        { label: "PaaS", description: "Vercel/Netlify/Heroku" },
        { label: "VPS/专用服务器", description: "自有服务器" },
        { label: "本地部署", description: "公司内部服务器" },
        { label: "未定", description: "尚未确定" }
      ]
    },
    {
      question: "关于开发・运维成本请告诉我",
      header: "预算约束",
      multiSelect: false,
      options: [
        { label: "成本最小化", description: "优先免费・低成本工具" },
        { label: "注重平衡", description: "可接受适度成本" },
        { label: "注重质量", description: "质量・效率优先于成本" },
        { label: "预算充足", description: "可选择最佳工具" }
      ]
    }
  ]
})
```

## 技术栈推荐逻辑

收集所有回答后,请根据以下逻辑推荐技术:

### 前端选择逻辑

```
IF 项目类型 == "Web应用程序" OR "全栈"
  IF 团队经验有 "React/Vue/Angular"
    IF 学习成本接受度 == "积极采用新技术"
      推荐: React 19 + TypeScript 5.7+ + Vite 6 (最新技术)
    ELSE
      推荐: React 18.3 + TypeScript 5.7+ + Vite 6 (注重稳定性)
  ELSE IF 有 "JavaScript/TypeScript" 经验
    推荐: Vue 3.5+ + TypeScript 5.7+ + Vite 6 (学习成本低)
  ELSE
    推荐: Next.js 15 + TypeScript 5.7+ (全栈简单)
```

### 后端选择逻辑

```
IF 有 "JavaScript/TypeScript" 丰富经验
  IF 学习成本接受度 == "积极采用新技术"
    推荐: Node.js 22 LTS + Fastify 5 + TypeScript 5.7+ (高速)
  ELSE
    推荐: Node.js 22 LTS + Express 5 + TypeScript 5.7+ (稳定)
ELSE IF 有 "Python" 丰富经验
  IF 性能需求 == "高负载"
    推荐: FastAPI 0.115+ (Python 3.12+) (高速API)
  ELSE
    推荐: Django 5.1+ (Python 3.12+) (全功能)
ELSE IF 有 "Java/Kotlin" 丰富经验
  推荐: Spring Boot 3.4+ (Kotlin 2.1+, Java 21+)
ELSE IF 有 "C#/.NET" 丰富经验
  推荐: ASP.NET Core 9 (.NET 9)
ELSE IF 有 "Go/Rust" 丰富经验
  推荐: Go 1.23+ (Gin 1.10+ / Fiber 3) 或 Rust 1.83+ (Actix-web 4)
ELSE
  推荐: Node.js 22 LTS + Express 5 (与前端统一)
```

### 数据库选择逻辑

```
IF 性能需求 == "高负载"
  推荐: PostgreSQL 17+ + Redis 7.4+ (可扩展性)
ELSE IF 安全需求 == "高级" OR "企业级"
  推荐: PostgreSQL 17+ (符合ACID、稳健)
ELSE IF 项目规模 == "个人开发" AND 开发周期 == "原型/MVP"
  推荐: SQLite 3.47+ → PostgreSQL 17+ (分阶段迁移)
ELSE
  推荐: PostgreSQL 17+ (通用)
```

### 开发环境・工具选择逻辑

```
容器:
  推荐: Docker 27+ + Docker Compose v2 (所有项目通用)

包管理器:
IF 语言 == "Python"
  推荐: uv (最快・现代化)
ELSE IF 语言 == "Node.js/TypeScript"
  推荐: pnpm 9+ (高速・磁盘效率高)
ELSE IF 语言 == "Java/Kotlin"
  推荐: Gradle 8.12+ (Kotlin DSL)

测试工具:
IF 前端 == React系 AND 构建工具 == Vite
  推荐: Vitest 2+ + Testing Library (高速集成)
ELSE IF 前端 == Vue系
  推荐: Vitest 2+ + Vue Test Utils
ELSE IF 后端 == Python
  推荐: pytest 8+ + pytest-asyncio
ELSE IF 后端 == Java/Kotlin
  推荐: JUnit 5 + Kotest

E2E测试:
  推荐: Playwright 1.49+ (支持所有语言・高速・高可靠性)

Linter・格式化工具:
IF 语言 == "Python"
  推荐: Ruff 0.8+ (最快全能)
ELSE IF 语言 == "TypeScript/JavaScript"
  推荐: Biome 1.9+ (最快) 或 ESLint 9+ + Prettier 3+
ELSE IF 语言 == "Kotlin"
  推荐: ktlint 1.5+ + detekt 1.23+
```

## 显示推荐结果

基于所有回答,请以下列格式用markdown显示推荐结果:

```markdown
# 基于您回答的技术栈推荐

## 📋 回答摘要
- 项目类型: [回答]
- 团队规模: [回答]
- 开发周期: [回答]
- 现有集成: [回答]
- 性能: [回答]
- 安全性: [回答]
- 技术技能: [回答(多个时用逗号分隔)]
- 学习成本接受度: [回答]
- 托管: [回答]
- 预算: [回答]

## 🚀 推荐技术栈

### 前端
✅ **[推荐技术]**
   - 理由: [选择理由]
   - 优点: [优点]
   - 学习成本: [低/中/高]

⚠️ **[备选方案]** (备选)
   - 理由: [选择理由]
   - 优点: [优点]
   - 学习成本: [低/中/高]

### 后端
✅ **[推荐技术]**
   - 理由: [选择理由]
   - 优点: [优点]
   - 学习成本: [低/中/高]

### 数据库
✅ **[推荐技术]**
   - 理由: [选择理由]
   - 优点: [优点]
   - 学习成本: [低/中/高]

### 开发环境・工具
✅ **[容器技术]**
✅ **[测试工具]**
✅ **[E2E测试]**
✅ **[Linter・格式化工具]**: ESLint + Prettier

## ⚙️ 一致性检查

✅ **技术选择的一致性**: [没有问题/需确认]
- [一致性说明]

✅ **与团队技能的匹配**: [良好/需要学习]
- [匹配状况说明]

✅ **与项目需求的适配性**: [适配/部分需调整]
- [适配性说明]
```

## 最终确认

显示推荐结果后,使用 AskUserQuestion 工具提出以下问题:

```
AskUserQuestion({
  questions: [
    {
      question: "是否按此推荐推进?",
      header: "最终确认",
      multiSelect: false,
      options: [
        { label: "是", description: "用此推荐生成 docs/tech-stack.md" },
        { label: "想部分修改", description: "调整个别技术" },
        { label: "重新开始", description: "从访谈重新开始" }
      ]
    }
  ]
})
```

### 选择"是"的情况

生成 `docs/tech-stack.md` 文件。模板如下:

```markdown
# 项目技术栈定义

## 🔧 生成信息
- **生成日期**: [YYYY-MM-DD]
- **生成工具**: init-tech-stack
- **项目类型**: [回答]
- **团队规模**: [回答]
- **开发周期**: [回答]

## 🎯 项目需求摘要
- **性能**: [回答]
- **安全性**: [回答]
- **技术技能**: [回答]
- **学习成本接受度**: [回答]
- **部署目标**: [回答]
- **预算**: [回答]

## 🚀 前端
- **框架**: [技术名称和版本]
- **语言**: [语言和版本]
- **状态管理**: [选择的工具]
- **打包工具**: [选择的工具]
- **路由**: [选择的工具]

### 选择理由
- [理由1]
- [理由2]
- [理由3]

## ⚙️ 后端
- **框架**: [技术名称和版本]
- **语言**: [语言和版本]
- **数据库**: [数据库名称和版本]
- **ORM**: [选择的工具]
- **认证**: [选择的方式]
- **缓存**: [选择的工具(如需要)]

### 选择理由
- [理由1]
- [理由2]
- [理由3]

## 💾 数据库设计
- **主DB**: [数据库名称和版本]
- **缓存**: [缓存工具(如需要)]
- **文件存储**: [存储策略]

### 设计方针
- [方针1]
- [方针2]
- [方针3]

## 🛠️ 开发环境
- **容器**: Docker + Docker Compose
- **包管理器**:
  - Python: uv (高速・现代化)
  - Node.js: pnpm (高速・磁盘效率高)
  - Java/Kotlin: Gradle (Kotlin DSL)
- **运行时版本**: [版本信息]

### 开发工具
- **测试框架**:
  - Python: pytest + pytest-asyncio
  - TypeScript/JavaScript: Vitest (使用Vite时) / Jest (其他)
  - Java/Kotlin: JUnit 5 + Kotest
- **E2E测试**: Playwright (支持所有语言・高速・高可靠性)
- **Linter・格式化工具**:
  - Python: Ruff (高速・全能)
  - TypeScript/JavaScript: Biome (高速) / ESLint + Prettier
  - Java/Kotlin: ktlint + detekt
- **类型检查**:
  - Python: mypy
  - TypeScript: tsc (TypeScript Compiler)

### CI/CD
- **CI/CD**: GitHub Actions (推荐)
- **代码质量**: ESLint, Prettier, [类型检查]
- **测试**: Unit, Integration, E2E
- **部署**: 带审批的自动部署

## ☁️ 基础设施・部署
- **前端**: [部署目标]
- **后端**: [部署目标]
- **数据库**: [DB托管]
- **缓存**: [缓存托管(如适用)]
- **CDN**: [CDN服务(如适用)]

## 🔒 安全性
- **HTTPS**: 必须 (证书自动更新)
- **认证**: [认证方式]
- **CORS**: 适当设置
- **验证**: 服务器端验证
- **环境变量**: 机密信息的适当管理
- **依赖关系**: 定期安全漏洞检查

## 📊 质量标准
- **测试覆盖率**: 80%以上
- **代码质量**: ESLint + Prettier
- **类型安全**: [类型安全要求]
- **性能**: Lighthouse 90+分(Web系统情况)
- **无障碍**: 符合WCAG 2.1 AA(Web系统情况)

## 📁 推荐目录结构

```
./ (当前目录 = 项目根目录)
├── frontend/                 # 前端
│   ├── src/
│   │   ├── components/      # React组件
│   │   ├── pages/           # 页面组件
│   │   ├── hooks/           # 自定义Hook
│   │   ├── utils/           # 工具函数
│   │   ├── types/           # TypeScript类型定义
│   │   ├── api/             # API客户端
│   │   └── App.tsx
│   ├── public/              # 静态文件
│   ├── tests/               # 测试
│   ├── package.json
│   ├── tsconfig.json
│   ├── vite.config.ts       # or next.config.js
│   └── tailwind.config.js
├── backend/                  # 后端
│   ├── app/                 # (Python/FastAPI/Django情况)
│   │   ├── main.py          # 入口点
│   │   ├── api/             # API路由
│   │   │   └── v1/
│   │   ├── core/            # 配置・安全
│   │   ├── models/          # ORM模型
│   │   ├── schemas/         # Schema定义
│   │   ├── services/        # 业务逻辑
│   │   ├── db/              # 数据库连接
│   │   └── utils/           # 工具
│   ├── src/                 # (Node.js/Java/Kotlin/Go等情况)
│   │   └── ...
│   ├── tests/               # 测试
│   ├── migrations/          # DB迁移
│   ├── pyproject.toml       # (Python) or package.json (Node.js) or build.gradle.kts (Kotlin)
│   └── Dockerfile
├── docker-compose.yml        # Docker配置
├── .github/
│   └── workflows/           # GitHub Actions
│       ├── ci.yml
│       └── deploy.yml
├── docs/                     # 文档
│   ├── tech-stack.md        # 本文件
│   ├── requirements/        # 需求定义书
│   ├── design/              # 设计书
│   └── tasks/               # 任务管理
├── .env.example             # 环境变量模板
├── .gitignore
└── README.md
```

**重要**: 上述的 `./` 表示当前目录(当前工作目录)。不是创建新目录,而是直接放置在现有项目根目录中。

## 🚀 安装步骤

### 1. 准备开发环境
```bash
[安装命令]
```

### 2. 主要命令
```bash
[开发所需的主要命令]
```

## 📝 自定义方法

请根据项目进展更新此文件:

1. **添加技术**: 添加新库・工具
2. **需求变更**: 更新性能・安全需求
3. **基础设施变更**: 变更部署目标・扩展需求
4. **团队变更**: 根据成员增减重新审视技术选择

## 🔄 更新历史
- [生成日期]: 初次生成 (由 init-tech-stack 自动生成)
```

生成后,显示以下确认消息:

```markdown
✅ 技术栈定义文件已生成!

📄 **生成文件**: `docs/tech-stack.md`
🎯 **推荐理由**: 与团队经验的适配性、对项目需求的优化

## 下一步

1. **确认文件**: 确认 `docs/tech-stack.md` 的内容
2. **自定义**: 根据需要微调技术选择
3. **团队共享**: 与团队达成技术选择共识
4. **开始开发**: 使用其他 kairo-* 命令进入需求定义・设计

此文件会被所有其他命令(kairo-*, tdd-*, direct-*)自动参考。
```

### 选择"想部分修改"的情况

使用 AskUserQuestion 工具提出以下问题:

```
AskUserQuestion({
  questions: [
    {
      question: "要修改哪个部分?",
      header: "自定义",
      multiSelect: true,
      options: [
        { label: "前端", description: "当前: [推荐技术]" },
        { label: "后端", description: "当前: [推荐技术]" },
        { label: "数据库", description: "当前: [推荐技术]" },
        { label: "开发环境・工具", description: "当前: [推荐技术]" }
      ]
    }
  ]
})
```

对于选择的部分,提供详细的单独选项,调整后再次进行最终确认。

### 选择"重新开始"的情况

从 Phase 1 重新开始访谈。

## 错误处理

### 处理现有文件

如果 `docs/tech-stack.md` 已存在,使用 AskUserQuestion 工具提出以下问题:

```
AskUserQuestion({
  questions: [
    {
      question: "docs/tech-stack.md 已存在。如何处理?",
      header: "现有文件",
      multiSelect: false,
      options: [
        { label: "覆盖", description: "覆盖现有文件" },
        { label: "备份后覆盖", description: "将现有文件保存为.bak" },
        { label: "另存为", description: "保存为 tech-stack-new.md" },
        { label: "取消", description: "中止处理" }
      ]
    }
  ]
})
```

## 重要注意事项

1. **使用 AskUserQuestion 工具**: 所有问题都请使用 AskUserQuestion 工具提出
2. **并行提问**: 请同时提出相关问题以提高效率
3. **保存回答**: 保存所有用户回答,用于最终文件生成
4. **确认最新版本**: 推荐技术栈时,请尽可能推荐最新稳定版本
5. **确认一致性**: 务必确认推荐技术栈之间的一致性