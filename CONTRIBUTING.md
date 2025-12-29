# 贡献指南

感谢您对 Tsumiki 项目的贡献!本指南将说明如何为项目做出贡献。

## 开发环境设置

### 必需环境

- Node.js 18.0.0 以上
- pnpm 10.13.1 以上

### 设置步骤

1. Fork 并克隆仓库:

```bash
git clone https://github.com/YOUR_USERNAME/tsumiki.git
cd tsumiki
```

2. 安装依赖:

```bash
pnpm install
```

3. 设置 pre-commit 钩子:

```bash
pnpm prepare
```

## 开发工作流

### 分支策略

- `main` 分支:稳定版本代码
- 功能开发:`feature/功能名`
- Bug 修复:`bugfix/Bug名`
- 热修复:`hotfix/修复内容`

### 开发步骤

1. 创建新分支:

```bash
git checkout -b feature/your-feature-name
```

2. 修改代码

3. 执行代码质量检查:

```bash
# 敏感信息检查
pnpm secretlint
```

4. 提交更改:

```bash
git add .
git commit -m "feat: 添加新功能"
```

## 提交信息规范

请使用 [Conventional Commits](https://www.conventionalcommits.org/) 格式:

- `feat:` 新功能
- `fix:` Bug 修复
- `docs:` 文档更改
- `style:` 代码样式更改(不影响功能)
- `refactor:` 重构
- `test:` 测试添加/修改
- `chore:` 构建流程或工具更改

示例:
```
feat: add new install command for .sh files
fix: resolve path handling issue in install command
docs: update README with new command examples
```

## 代码质量标准

### 自动检查

Pre-commit 钩子会自动执行以下操作:

- **secretlint**: 检查敏感信息(API 密钥、密码等)是否混入

### 手动检查

更改前请执行以下命令:

```bash
# 敏感信息检查
pnpm secretlint
```

## 项目结构

```
tsumiki/
├── .claude-plugin/         # Claude Code Plugin 配置
├── commands/               # 命令模板(.md, .sh)
├── agents/                 # 代理定义(.md)
├── book/                   # 文档
├── package.json
├── CLAUDE.md              # 项目说明书
└── README.md
```

## Pull Request

### 创建 Pull Request

1. 推送更改:

```bash
git push origin feature/your-feature-name
```

2. 在 GitHub 上创建 Pull Request

3. 按照 Pull Request 模板填写说明

### Pull Request 要求

- [ ] 更改内容有明确说明
- [ ] 已链接相关 Issue(如适用)
- [ ] 代码质量检查已通过
- [ ] 不包含敏感信息

## Issue 报告

我们在 [Issues](https://github.com/classmethod/tsumiki/issues) 接受 Bug 报告和功能建议。

### Bug 报告

请包含以下信息:

- 重现步骤
- 预期行为
- 实际行为
- 环境信息(OS、Node.js 版本等)
- 错误消息(如适用)

### 功能建议

请包含以下信息:

- 建议功能的说明
- 使用场景
- 预期收益
- 实现方案(如有)

## 安全性

如果发现安全相关问题,请私下报告,不要在公开 Issue 中发布。

## 许可证

本项目在 MIT 许可证下发布。贡献时将被视为同意此许可证。

## 问题与支持

- [Issues](https://github.com/classmethod/tsumiki/issues) - Bug 报告、功能建议
- [Discussions](https://github.com/classmethod/tsumiki/discussions) - 问题、讨论

期待您的贡献!