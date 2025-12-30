---
description: 对DIRECT任务执行的配置工作进行操作确认和测试。确认配置已正确应用，系统按预期运行。
allowed-tools: Read, Glob, Grep, Task, Write, Edit, TodoWrite
argument-hint: [需求名称] [TASK-ID]
---

# direct-verify

## 目的

对DIRECT任务执行的配置工作进行操作确认和测试。确认配置已正确应用，系统按预期运行。

## 前提条件

- `/jimu:direct-setup` 已执行完毕
- 已提供任务ID
- 配置工作记录已存在

## 执行内容

**【重要】**: 对于direct-setup创建的文件，如果发现编译错误或语法错误，会自动尝试解决。

### 1. 加载附加规则
- 如果存在 `docs/rule` 目录则加载
- 如果存在 `docs/rule/direct` 目录则加载
- 如果存在 `docs/rule/direct/verify` 目录则加载
- 读取各目录中的所有文件，作为附加规则应用

### 2. 加载技术栈定义
- 如果存在 `docs/tech-stack.md` 则加载
- 如果不存在，则从 `CLAUDE.md` 中读取技术栈部分
- 如果两者都不存在，则使用 `.claude/commands/tech-stack.md` 的默认定义

### 3. 配置确认
- 根据加载的技术栈定义确定验证项目
- 使用 @agent-symbol-searcher 搜索相关配置和验证模式，用Read工具读取找到的文件
- 用Read工具读取 `docs/implements/{需求名称}/{TASK-ID}/setup-report.md`，确认配置工作结果
- 确认环境变量
- 确认配置文件内容
- 确认依赖安装状态
- 确认服务启动状态

### 4. 编译·语法确认
- TypeScript/JavaScript语法错误检查(如适用)
- 配置文件语法确认(JSON, YAML等)
- SQL语法确认(如适用)
- 消除最基本的编译错误
- **如果发现错误，会自动尝试修复**

### 5. 执行操作测试
- 使用 @agent-symbol-searcher 搜索现有测试用例和验证脚本，用Read工具读取找到的文件
- 基本操作确认
- 连接测试
- 权限确认
- 错误用例确认

### 6. 质量检查
- 确认安全配置
- 确认性能标准
- 确认日志

### 7. 记录到CLAUDE.md
- 确认子项目存在(如 `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod` 等)
- 确认各子项目或根目录的 `CLAUDE.md` 是否存在
- 确认CLAUDE.md中是否**最小限度**记录了以下信息:
  - 测试执行命令
  - 应用启动命令
  - 构建命令(如适用)
  - 数据库操作命令(如适用)
- 如果信息不足，则添加/更新「## 开发命令」部分
- 如果现有信息过时则更新。如果文件不存在，根据需要新建

### 8. 创建验证报告
- 创建 `docs/implements/{需求名称}/{TASK-ID}/verify-report.md`
- 记录所有确认结果
- 记录发现的问题和解决内容
- 记录CLAUDE.md的记录内容
- 记录建议事项和下一步

### 9. 任务完成标记
如果满足所有完成条件，**自动**更新以下文件:

#### 完成条件
- 所有配置确认项目已通过
- 编译·语法检查成功(所有错误已解决)
- 所有操作测试成功
- 质量检查项目符合标准
- 发现的问题已适当处理
- 安全配置适当
- 满足性能标准

#### 更新目标文件
1. **Overview 文件**: `docs/tasks/{需求名称}/overview.md` 或 `docs/tasks/{需求名称}-overview.md`
   - 将相关任务的进度状态更新为「完成」
   - 记录完成日期

2. **任务详细文件**: `docs/tasks/{需求名称}/TASK-{task_id}.md` 或 `docs/tasks/{需求名称}-tasks.md`
   - 将状态更新为「✅ 完成」
   - 记录完成日期
   - 将所有完成条件的复选框更改为 `[x]`

## 输出位置

确认记录将在 `docs/implements/{需求名称}/{TASK-ID}/` 目录下创建以下文件:
- `verify-report.md`: 配置确认·操作测试记录

## 记录到CLAUDE.md

操作确认中需要的最小执行方法，**必须记录到CLAUDE.md**:

### 应记录的内容(最小限度)
- **测试用例执行方法**
  - 测试命令(例: `npm test`, `pytest`, `cargo test`)
  - 特定测试文件/目录的执行方法
  - 测试环境设置方法(仅在必要时)

- **应用执行方法**
  - 开发服务器启动命令(例: `npm run dev`, `python manage.py runserver`)
  - 构建命令(例: `npm run build`, `cargo build --release`)
  - 生产环境启动方法(如适用)

- **数据库相关**(仅在适用时)
  - 迁移执行方法
  - 种子数据注入方法

### 记录位置判定逻辑

1. **确定子项目**
   - 根据 `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod` 等的存在来判定
   - 为每个子项目创建独立的CLAUDE.md

2. **CLAUDE.md的放置规则**
   - **存在子项目的情况**: 在各子项目的根目录创建 `CLAUDE.md`
     - 例: `frontend/CLAUDE.md`, `backend/CLAUDE.md`
   - **Monorepo结构的情况**: 在根目录的 `CLAUDE.md` 中记录整体概况，在各子项目中记录详细信息
   - **单一项目的情况**: 仅在根目录的 `CLAUDE.md` 中记录

3. **存在现有CLAUDE.md的情况**
   - 检查是否已记录必要信息
   - 仅追加缺少的信息
   - 如果信息过时则更新

### 记录方法

在各CLAUDE.md中添加或更新以下部分:

```markdown
## 开发命令

### 测试执行
\`\`\`bash
# 执行所有测试
{测试命令}

# 执行特定测试
{特定测试执行命令}
\`\`\`

### 应用执行
\`\`\`bash
# 启动开发服务器
{开发服务器启动命令}

# 构建
{构建命令}
\`\`\`

### 数据库操作(如适用)
\`\`\`bash
# 执行迁移
{迁移命令}

# 注入种子数据
{种子命令}
\`\`\`
```

### 记录优先级

1. **必需**: 测试执行方法、应用启动方法
2. **推荐**: 构建方法、数据库操作(如适用)
3. **可选**: 详细故障排除(记录到README.md)

## 记录到README.md

操作确认结果和与运营相关的重要信息，**也必须记录到README.md**:

### 应记录的内容
- 操作确认步骤和命令
- 各组件正常运行的确认方法
- 性能基准值(启动时间、内存使用量等)
- 发现的问题和解决方法(故障排除)
- 推荐的运营配置
- 安全相关注意事项

### 记录方法
1. 在**操作确认部分**追加确认步骤
   - 如果已有「操作确认」部分，则追加到那里
   - 如果没有则新建

2. 在**故障排除部分**追加问题和解决方法
   - 必须记录发现的问题
   - 解决命令要以可执行的形式记录

3. 在**项目状态部分**更新任务完成信息
   - 任务完成时会自动更新

### 记录例

#### 向动作确认部分添加内容
```markdown
### {服务名}的验证

\`\`\`bash
# {确认项目的说明}
{确认命令}
\`\`\`

**期望结果**: {期望的输出或状态}
```

#### 性能基准的记录
```markdown
### 性能基准

- 启动时间: {时间}秒以内
- 内存使用量: {大小}MB以内
- 响应时间: {时间}ms以内
```

#### 向故障排除部分添加内容
```markdown
### {问题概述}

**症状**: {问题的详细说明}

\`\`\`bash
# 问题确认
{确认命令}

# 解决方法
{解决命令}
\`\`\`

**原因**: {问题的原因}
**解决**: {解决方案的说明}
```

## 输出格式示例

<verify_report_format>
```markdown
# {TASK-ID} 配置确认·动作测试

## 确认概要

- **任务ID**: {TASK-ID}
- **确认内容**: {配置确认的概要}
- **执行时间**: {执行时间}
- **执行者**: {执行者}

## 配置确认结果

### 1. 环境变量的确认

```bash
# 执行的命令
echo $NODE_ENV
echo $DATABASE_URL
```
```

**确认结果**:

- [x] NODE_ENV: development (期望值: development)
- [x] DATABASE_URL: postgresql://localhost:5432/mydb (期望值: 正确的DB URL)

### 2. 配置文件的确认

**确认文件**: `config/database.json`

```bash
# 执行的命令
cat config/database.json | jq .
```

**确认结果**:

- [x] 文件存在
- [x] JSON格式正确
- [x] 包含必需的配置项

## 编译·语法检查结果

### 1. TypeScript/JavaScript语法检查

```bash
# 如果存在TypeScript文件
npx tsc --noEmit --skipLibCheck

# JavaScript语法检查
node --check *.js
```

**检查结果**:

- [x] TypeScript语法错误: 无
- [x] JavaScript语法错误: 无
- [x] import/require语句: 正常

### 2. 配置文件语法检查

```bash
# JSON配置文件的语法检查
cat config/*.json | jq empty

# YAML配置文件的语法检查(如适用)
yamllint config/*.yml
```

**检查结果**:

- [x] JSON语法: 正常
- [x] YAML语法: 正常(如适用)
- [x] 配置项的合理性: 已确认

### 3. SQL语法检查(如适用)

```bash
# SQL语法的基本检查
psql -d mydb --single-transaction --set ON_ERROR_STOP=on -f schema.sql --dry-run
```

**检查结果**:

- [x] SQL语法: 正常
- [x] 表定义: 正常
- [x] 约束定义: 正常

### 3. 依赖关系的确认

```bash
# 执行的命令
npm list express pg
```

**确认结果**:

- [x] express: 已安装
- [x] pg: 已安装

### 4. 数据库连接测试

```bash
# 执行的命令
psql -d mydb -c "SELECT 1;"
```

**确认结果**:

- [x] 数据库连接成功
- [x] 查询执行成功

## 动作测试结果

### 1. 基本动作测试

```bash
# 执行的测试命令
node -e "console.log('Hello, World!');"
```

**测试结果**:

- [x] Node.js执行环境: 正常
- [x] 基本JavaScript执行: 正常

### 2. 数据库连接测试

```javascript
// 测试脚本
const { Pool } = require('pg');
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

pool.query('SELECT NOW()', (err, res) => {
  if (err) {
    console.error('Error:', err);
  } else {
    console.log('Connected:', res.rows[0]);
  }
  pool.end();
});
```

**测试结果**:

- [x] 数据库连接: 正常
- [x] 查询执行: 正常
- [x] 连接关闭: 正常

### 3. 安全配置测试

```bash
# 执行的命令
ls -la config/
ps aux | grep node
```

**测试结果**:

- [x] 配置文件权限: 恰当
- [x] 进程执行权限: 恰当
- [x] 机密信息保护: 恰当

## 质量检查结果

### 性能确认

- [x] 启动时间: 2秒以内
- [x] 内存使用量: 256MB以内
- [x] CPU使用率: 10%以内

### 日志确认

- [x] 错误日志: 无异常
- [x] 警告日志: 无问题
- [x] 信息日志: 适当输出

## 总体确认结果

- [x] 配置工作正确完成
- [x] 全部动作测试成功
- [x] 质量标准得到满足
- [x] 准备好进入下一任务

## 发现的问题与解决

### 语法错误·编译错误的解决

**尝试自动解决的问题**:
- TypeScript/JavaScript语法错误
- JSON/YAML语法错误
- 基本SQL语法错误
- import/require语句问题

### 问题1: {如有问题则记载}

- **问题内容**: {问题的详细}
- **发现方法**: {语法检查/编译/动作测试}
- **重要程度**: {高/中/低}
- **自动解决**: {执行的解决命令·修正内容}
- **解决结果**: {已解决/需要手动对应}

### 解决执行日志

```bash
# 执行的解决命令示例
# 语法错误修正
sed -i 's/typo/correct/g' config.js

# 依赖关系修正
npm install missing-package

# 配置文件修正
jq '.port = 3000' config.json > temp.json && mv temp.json config.json
```

**解决结果**:
- [x] 问题1: 已解决
- [x] 问题2: 已解决
- [ ] 问题3: 需要手动对应(详情见推荐事项)

## 推荐事项

- {如有改进建议则记载}
- {如有优化建议则记载}

## 下一步骤

- 任务完成报告
- 相关任务的开始准备
- 必要时进行配置微调

## 记录到CLAUDE.md的内容

### 更新对象
- {子项目路径}/CLAUDE.md(如果存在子项目)
- ./CLAUDE.md(单一项目的情况)

### 添加的信息
```markdown
## 开发命令

### 测试执行
\`\`\`bash
# 执行所有测试
{实际使用的测试命令}

# 执行特定测试
{实际使用的特定测试执行命令}
\`\`\`

### 应用执行
\`\`\`bash
# 启动开发服务器
{实际使用的启动命令}

# 构建
{实际使用的构建命令}
\`\`\`
```

### 更新理由
- {CLAUDE.md中不存在该信息/信息过时}
- 记录动作确认中必需的最小执行方法

```
</verify_report_format>

## 更新文件的格式

### Overview文件的更新格式
<overview_update_format>
```markdown
# 进展摘要
## 阶段1: {阶段名}
- [x] TASK-0001: {任务名} ✅ 完成 (2025-11-03)
- [ ] TASK-0002: {任务名}
```
</overview_update_format>

### 任务详细文件的更新格式
<task_detail_update_format>
```markdown
## TASK-0001: {任务名}

### 状态
- **状态**: ✅ 完成
- **完成日期**: 2025-11-03
- **实现者**: Claude

### 完成条件
- [x] 全部配置确认项目已通过
- [x] 编译·语法检查成功
- [x] 全部动作测试成功
- [x] 质量检查项目符合标准
```
</task_detail_update_format>
