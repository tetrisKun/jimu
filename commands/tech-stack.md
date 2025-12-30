# 默认技术栈定义

在找不到项目专用的技术栈定义文件时使用的默认定义。

## 优先级

1. **项目专用**: `docs/tech-stack.md`
2. **项目通用**: `CLAUDE.md` 的技术栈部分
3. **默认**: 本定义文件

## 默认技术栈

### 前端
- **框架**: React 18+ 或 Vue 3+ 或 Next.js
- **语言**: TypeScript 5.0+
- **状态管理**: Redux Toolkit 或 Zustand 或 Pinia
- **UI库**: Material-UI 或 Tailwind CSS 或 shadcn/ui
- **打包工具**: Vite 或 Webpack

### 后端
- **框架**: Express.js 或 Fastify 或 Next.js API Routes
- **语言**: TypeScript 5.0+ 或 JavaScript ES2022+
- **数据库**: PostgreSQL 15+ 或 MySQL 8+ 或 SQLite
- **ORM**: Prisma 或 TypeORM 或 Drizzle
- **认证**: JWT 或 NextAuth.js

### 缓存・会话
- **缓存**: Redis 7+ 或 Memcached
- **会话**: Redis 或 MemoryStore

### 开发环境
- **容器**: Docker + Docker Compose
- **包管理器**: npm 或 yarn 或 pnpm
- **Node.js**: 18+ LTS

### 开发工具
- **测试框架**: Jest 或 Vitest
- **测试库**: Testing Library 或 Playwright
- **代码检查**: ESLint + Prettier
- **类型检查**: TypeScript
- **CI/CD**: GitHub Actions 或 GitLab CI

### 部署・基础设施
- **前端**: Vercel 或 Netlify 或 Cloudflare Pages
- **后端**: Railway 或 Heroku 或 AWS 或 GCP
- **数据库**: PostgreSQL (托管型) 或 自管理
- **CDN**: Cloudflare 或 AWS CloudFront

## API 设计
- **架构**: RESTful API 或 GraphQL
- **文档**: OpenAPI/Swagger 或 GraphQL Schema
- **认证方式**: Bearer Token (JWT) 或 API Key

## 数据管理
- **数据库设计**: 规范化 + 必要时非规范化
- **迁移**: Prisma Migrate 或 TypeORM Migrations
- **备份**: 推荐自动备份

## 安全性
- **HTTPS**: 必需
- **CORS**: 适当配置
- **认证**: JWT + Refresh Token 模式
- **验证**: 服务器端验证必需
- **环境变量**: 机密信息的适当管理

## 性能要求
- **API 响应时间**: 3秒以内
- **前端首屏加载**: 2秒以内
- **数据库查询**: 索引优化
- **缓存策略**: 适当的 TTL 设置

## 质量标准
- **测试覆盖率**: 80%以上推荐
- **代码质量**: ESLint + Prettier
- **类型安全**: TypeScript strict mode
- **无障碍**: 符合 WCAG 2.1 AA 推荐

## 目录结构（推荐）

```
project/
├── docs/                    # 文档
│   ├── spec/               # 需求定义
│   ├── design/             # 设计文档
│   └── tasks/              # 任务管理
├── src/                    # 源代码
│   ├── components/         # UI 组件
│   ├── services/           # 业务逻辑
│   ├── types/              # 类型定义
│   └── utils/              # 工具函数
├── tests/                  # 测试文件
├── prisma/                 # 数据库模式
├── docker-compose.yml      # 开发环境
└── package.json           # 依赖关系
```

## 使用方法

以下情况下会参考此默认定义：

1. 不存在 `docs/tech-stack.md`
2. `CLAUDE.md` 中无技术栈信息
3. 新建项目的初始设置

如果项目有特定的技术选择，请创建 `docs/tech-stack.md` 来覆盖此定义。