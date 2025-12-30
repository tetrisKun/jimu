---
description: 进行TDD开发的需求整理。明确功能需求,为测试驱动开发做准备。
allowed-tools: Read, Glob, Grep, Task, Write, TodoWrite, Edit
argument-hint: [需求名称] [TASK-ID]
---
实施TDD开发的需求整理,参照EARS需求定义书・设计文档明确功能规格。显示可靠性级别创建需求定义。

# context

输出目录="./docs/implements"
功能名={{feature_name}}
任务ID={{task_id}}
需求名称={{requirement_name}}
可靠性评估=[]
任务文件1=./docs/tasks/{需求名称}/{{task_id}}.md
任务文件2=./docs/tasks/{需求名称}-phase*.md
任务overview文件1=./docs/tasks/{需求名称}/overview.md
任务overview文件2=./docs/tasks/{需求名称}-overview.md

# step

- 如果没有 $ARGUMENTS,则说"请在参数中指定需求名称和TASK-ID(例: 用户认证功能 TASK-0001)"后结束
- 向用户声明 $ARGUMENTS 的内容和 context 的内容
- 执行 step2

## step2

- 执行开发上下文的准备:
  **读取任务笔记**
  - 如果存在 `./docs/implements/{需求名称}/{{task_id}}/note.md` 则读取
  - 如果不存在:
    - 使用 @task 执行 `/jimu:tdd-tasknote {需求名称} {{task_id}}` 命令生成笔记
    - 读取生成的笔记文件
  - 笔记包含技术栈、开发规则、相关实现、设计文档、注意事项

- 读取完成后,执行 step3

## step3

- 用 context 的信息填充 <requirements_template> 的内容,直接创建需求定义书
  - 利用读取的上下文信息(任务笔记、附加规则等)
  - 在各项目中记载可靠性级别(🔵🟡🔴)
  - 使用 Write 工具保存到输出文件

- 根据质量判定标准评估创建的需求定义书的内容:
  - 需求的模糊性有无
  - 输入输出定义的完整性
  - 约束条件的明确性
  - 实现可能性
  - 可靠性级别(🔵🟡🔴的分布)

- 向用户显示质量判定结果
- 执行 step4

## step4

- 使用 TodoWrite 工具更新 TODO 状态
  - 将当前TODO标记为"completed"
  - 在TODO内容中反映需求定义阶段的完成
  - 将下一阶段"测试用例洗出"添加到TODO
  - 在TODO内容中记录质量判定结果
- 显示下一步: "推荐的下一步: 使用 `/jimu:tdd-testcases {{需求名称}} {{TASK-ID}}` 进行测试用例的洗出。"

# rules

## 文件名规则

### 输出文件的路径格式
- `docs/implements/{需求名称}/{task_id}/{feature_name}-requirements.md`
- 例: `docs/implements/user-auth/task-001/login-requirements.md`

### 文件名命名规则
- 将功能名转换为简洁的英语
- 使用短横线命名法(kebab-case)
- 控制在50个字符左右
- 例:
  - "用户认证功能" → "user-auth"
  - "数据导出功能" → "data-export"
  - "密码重置功能" → "password-reset"

## 质量判定标准

```
✅ 高质量:
- 需求的模糊性: 无
- 输入输出定义: 完整
- 约束条件: 明确
- 实现可能性: 确实
- 可靠性级别: 🔵(绿灯)较多

⚠️ 需改进:
- 需求有模糊部分
- 输入输出的详细不明确
- 技术限制不明
- 需要确认用户意图
- 可靠性级别: 🟡🔴(黄灯、红灯)较多
```

## TODO更新模式

```
- 将当前TODO标记为"completed"
- 在TODO内容中反映需求定义阶段的完成
- 将下一阶段"测试用例洗出"添加到TODO
- 在TODO内容中记录质量判定结果
```

## 可靠性级别指示

关于各项目,请用以下信号注释与原始资料(含EARS需求定义书・设计文档)的对照情况:

- 🔵 **绿灯**: 参考EARS需求定义书・设计文档几乎没有推测的情况
- 🟡 **黄灯**: 从EARS需求定义书・设计文档进行合理推测的情况
- 🔴 **红灯**: EARS需求定义书・设计文档中没有的推测情况

## 文件路径记载规则

- **使用以项目根目录为基准的相对路径**
- 不记载全路径(绝对路径)
- 例:
  - ❌ `/Users/username/projects/myapp/src/utils/helper.ts`
  - ✅ `src/utils/helper.ts`

# info

<requirements_template>
您是TDD开发的需求整理负责人。请根据以下信息明确功能需求。

# 需求整理对象的信息

功能名: {{功能名}}
任务ID: {{任务ID}}
需求名称: {{需求名称}}
输出文件名: {{输出文件名}}

# 重要的注意事项

**所有文件路径请以项目根目录为基准用相对路径记载。**
**请勿使用绝对路径(/Users/... 或 C:\... 等)。**

例:
- ❌ `/Users/username/projects/myapp/src/utils/helper.ts`
- ✅ `src/utils/helper.ts`

# 需求整理步骤

请利用已读取的以下上下文信息:
- **任务笔记**(`note.md`)
  - 技术栈(使用技术・框架・架构模式)
  - 开发规则(编码规范・类型检查・测试要求)
  - 相关实现(现有的实现模式・参考代码)
  - 设计文档(数据模型・目录结构)
  - 注意事项(技术限制・安全要求・性能要求)
- 附加规则(`docs/rule`, `docs/rule/tdd`, `docs/rule/tdd/requirements`)
- 现有的需求定义・测试用例

基于这些信息,请按以下格式整理需求。

## TDD用需求整理格式

**【可靠性级别指示】**:
关于各项目,请用以下信号注释与原始资料(含EARS需求定义书・设计文档)的对照情况:

- 🔵 **绿灯**: 参考EARS需求定义书・设计文档几乎没有推测的情况
- 🟡 **黄灯**: 从EARS需求定义书・设计文档进行合理推测的情况
- 🔴 **红灯**: EARS需求定义书・设计文档中没有的推测情况

### 1. 功能的概要(EARS需求定义书・设计文档基础)

- 🔵🟡🔴 记载各项目的可靠性级别
- 什么功能(从用户故事提取)
- 解决什么样的问题(从As a / So that提取)
- 假定的用户(从As a提取)
- 系统内的定位(从架构设计提取)
- **参照的EARS需求**: [具体的需求ID]
- **参照的设计文档**: [architecture.md的相应部分]

### 2. 输入・输出的规格(EARS功能需求・TypeScript类型定义基础)

- 🔵🟡🔴 记载各项目的可靠性级别
- 输入参数(类型、范围、约束) - 从interfaces.ts提取
- 输出值(类型、格式、例) - 从interfaces.ts提取
- 输入输出的关系性
- 数据流(从dataflow.md提取)
- **参照的EARS需求**: [具体的REQ-XXX]
- **参照的设计文档**: [interfaces.ts的相应接口]

### 3. 约束条件(EARS非功能需求・架构设计基础)

- 🔵🟡🔴 记载各项目的可靠性级别
- 性能要求(从NFR-XXX提取)
- 安全要求(从NFR-XXX提取)
- 兼容性要求(从REQ-XXX MUST提取)
- 架构约束(从architecture.md提取)
- 数据库约束(从database-schema.sql提取)
- API约束(从api-endpoints.md提取)
- **参照的EARS需求**: [具体的NFR-XXX, REQ-XXX]
- **参照的设计文档**: [architecture.md, database-schema.sql等的相应部分]

### 4. 假定的使用例(EARS Edge案例・数据流基础)

- 🔵🟡🔴 记载各项目的可靠性级别
- 基本的使用模式(从通常需求REQ-XXX提取)
- 数据流(从dataflow.md提取)
- 边缘案例(从EDGE-XXX提取)
- 错误案例(从EDGE-XXX错误处理提取)
- **参照的EARS需求**: [具体的EDGE-XXX]
- **参照的设计文档**: [dataflow.md的相应流程图]

### 5. EARS需求・设计文档的对应关系

参照了现有的EARS需求定义书・设计文档的情况下,请明记以下对应关系:

- **参照的用户故事**: [故事名]
- **参照的功能需求**: [REQ-001, REQ-002, ...]
- **参照的非功能需求**: [NFR-001, NFR-101, ...]
- **参照的Edge案例**: [EDGE-001, EDGE-101, ...]
- **参照的接受标准**: [具体的测试项目]
- **参照的设计文档**:
  - **架构**: [architecture.md的相应部分]
  - **数据流**: [dataflow.md的相应流程图]
  - **类型定义**: [interfaces.ts的相应接口]
  - **数据库**: [database-schema.sql的相应表]
  - **API规格**: [api-endpoints.md的相应端点]

# 输出格式

请按上述格式创建需求定义书。

请务必使用 Write 工具将结果保存到 {{输出文件名}}。
</requirements_template>
