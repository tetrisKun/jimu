# Jimu - 基于 Tsumiki 中文化的 AI 驱动开发支持框架

Jimu 是一个基于 Tsumiki 中文化的用于 AI 驱动开发的框架。提供从需求定义到实现,利用 AI 的高效开发流程。

基本上支持 Claude Code,但也可以与其他工具一起使用。请参考 [在 Claude Code 以外的工具中使用 tsumiki](#在-claude-code-以外的工具中使用-tsumiki)。

## 安装

要使用 Jimu,请使用以下 Claude Code Plugin 命令安装:

```bash
/plugin marketplace add https://github.com/tetrisKun/jimu.git
/plugin install jimu@jimu
```

执行此命令后,Jimu 的 Claude Code 斜杠命令和代理将自动安装。

**注意**: 命令使用 `/jimu:` 前缀执行(例: `/jimu:kairo-requirements`)。

## 概述

Tsumiki 由以下 2 个命令组成:

- **kairo** - 从需求定义到实现的综合开发流程
- **tdd** - 测试驱动开发(TDD)的单独执行

### Kairo 命令

Kairo 自动化·支持从需求定义到实现的开发流程。支持以下开发流程:

1. **需求定义** - 从概要生成详细的需求定义书
2. **设计** - 自动生成技术设计文档
3. **任务分割** - 适当分割·排序实现任务
4. **TDD 实现** - 通过测试驱动开发实现高质量

## 可用命令

- `init-tech-stack` - 识别技术栈

### Kairo 命令(综合开发流程)
- `kairo-requirements` - 需求定义
- `kairo-design` - 生成设计文档
- `kairo-tasks` - 任务分割
- `kairo-implement` - 执行实现

### TDD 命令(单独执行)
- `tdd-requirements` - TDD 需求定义
- `tdd-testcases` - 创建测试用例
- `tdd-red` - 测试实现(Red)
- `tdd-green` - 最小实现(Green)
- `tdd-refactor` - 重构
- `tdd-verify-complete` - TDD 完成确认

### 逆向工程命令
- `rev-tasks` - 从现有代码逆向生成任务列表
- `rev-design` - 从现有代码逆向生成设计文档
- `rev-specs` - 从现有代码逆向生成测试规范
- `rev-requirements` - 从现有代码逆向生成需求定义书

## 快速开始

**注意**: 如果使用 Claude Code Plugin 安装,请在每个命令前添加 `tsumiki:`(例: `/jimu:kairo-requirements`)。

### 综合开发流程

```bash
# 1. 技术栈初始化
/jimu:init-tech-stack

# 2. 需求定义
/jimu:kairo-requirements

# 3. 设计
/jimu:kairo-design

# 4. 任务分割
/jimu:kairo-tasks

# 5. 实现
/jimu:kairo-implement
```

### 单独 TDD 流程

```bash
/jimu:tdd-requirements
/jimu:tdd-testcases
/jimu:tdd-red
/jimu:tdd-green
/jimu:tdd-refactor
/jimu:tdd-verify-complete
```

### 逆向工程

```bash
# 1. 从现有代码分析任务结构
/jimu:rev-tasks

# 2. 逆向生成设计文档(建议在任务分析后执行)
/jimu:rev-design

# 3. 逆向生成测试规范(建议在设计文档后执行)
/jimu:rev-specs

# 4. 逆向生成需求定义书(建议在完成所有分析后执行)
/jimu:rev-requirements
```

## 在 Claude Code 以外的工具中使用 tsumiki

通过结合使用 [rulesync](https://github.com/dyoshikawa/rulesync),也可以在 Claude Code 以外的工具中使用 tsumiki 命令。

在项目根目录执行以下命令。

```
npx -y rulesync init
npx -y rulesync config --init
npx -y rulesync import \
  --targets claudecode \
  --features commands,subagents

# 如果要输出 Gemini CLI 的自定义斜杠命令,如下所示。
# (`--targets` 可以指定 `claudecode`, `geminicli`, `roo`)
npx -y rulesync generate \
  --targets geminicli \
  --features commands

# 即使对于不存在自定义斜杠命令规范(或有规范限制)的 AI 编码工具,通过 `--experimental-simulate-commands` 标志,某些工具也可以输出命令文件。
# 如果要输出 Cursor 的自定义斜杠命令,如下所示。
# (`--targets` 可以指定 `cursor`, `copilot`, `codexcli`)
npx -y rulesync generate \
  --targets cursor \
  --features commands
  --experimental-simulate-commands
```

详情请参考 [rulesync](https://github.com/dyoshikawa/rulesync) 的 README。

## 详细手册

有关使用方法的详细信息、目录结构、工作流示例、故障排除,请参考 [MANUAL.md](./MANUAL.md)。