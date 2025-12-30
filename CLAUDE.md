# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

Jimu 是基于 Tsumiki 中文化的一个 AI 驱动的开发框架。通过 Claude Code Plugin 安装,提供从需求定义到实现的 AI 辅助开发流程。

此仓库包含以下内容:
- **`commands/`**: Claude Code 斜杠命令的模板文件(`.md` 和 `.sh`)
- **`agents/`**: Claude Code 代理的定义文件(`.md`)
- **`.claude-plugin/`**: Claude Code Plugin 配置文件

## 开发命令

```bash
# 开发环境
pnpm install                # 安装依赖

# 代码质量
pnpm secretlint             # 检查敏感信息

# pre-commit 钩子
pnpm prepare                # 设置 simple-git-hooks
```

## 项目结构

- **`commands/`**: Tsumiki AI 开发框架的 Claude Code 命令模板(`.md` 和 `.sh` 文件)
- **`agents/`**: Claude Code 代理定义(`.md` 文件)
- **`.claude-plugin/`**: Claude Code Plugin 配置(marketplace.json, plugin.json)
- **`book/`**: 开发指南和文档

## 技术栈

- **Security**: secretlint(敏感信息检查)
- **Package Manager**: pnpm
- **Distribution**: Claude Code Plugin Marketplace

## 安装方法

用户可以使用以下命令安装 Jimu:

```bash
/plugin marketplace add https://github.com/tetrisKun/jimu.git
/plugin install jimu@jimu
```

Claude Code Plugin 会自动:
1. 从仓库加载 `commands/` 和 `agents/` 的文件
2. 根据 `.claude-plugin/plugin.json` 的配置注册命令和代理
3. 使命令可以使用 `/jimu:` 前缀

## 质量管理

Pre-commit 钩子会自动执行以下操作:
- `pnpm secretlint`: 检查敏感信息

修改命令文件(`.md`)或代理定义(`.md`)时,请在提交前确认不包含敏感信息。