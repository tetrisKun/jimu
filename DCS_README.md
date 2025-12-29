# DCS 文档

## 整体概述

DCS 是一个命令套件,用于支持软件开发各个阶段的各种分析和规划工作。每个命令都以交互方式收集必要的信息,并在 `.dcs/` 目录下输出结构化的分析结果。

### 按开发阶段分类的命令列表

#### **企划·需求定义阶段**
- `/tsumiki:dcs:feature-rubber-duck` - 整理想法并创建 PRD

#### **设计·分析阶段**
- `/tsumiki:dcs:sequence-diagram-analysis` - 创建序列图
- `/tsumiki:dcs:state-transition-analysis` - 状态转换分析
- `/tsumiki:dcs:impact-analysis` - 影响范围分析

#### **实现计划阶段**
- `/tsumiki:dcs:incremental-dev` - 增量开发计划

#### **调试·维护阶段**
- `/tsumiki:dcs:bug-analysis` - Bug 原因分析
- `/tsumiki:dcs:performance-analysis` - 性能问题调查

---

## 命令详情

### 1. feature-rubber-duck

**用途**
通过对话整理模糊的想法或需求,创建可实现的 PRD(产品需求文档)。

**概述**
像橡皮鸭调试法一样,通过重复对话将功能想法具体化。逐步推进代码库调查、技术验证、需求具体化,最终创建可实现的 PRD。

**使用时机**
- 想整理新功能的想法时
- 功能需求模糊需要具体化时
- 想探讨如何集成到现有代码库时
- 想在验证技术可行性的同时完善需求时

**主要输出文件**
```
.dcs/{{timestamp}}_{{feature_name}}/
├── index.md              # 会话信息索引
├── conversation.md       # 对话历史
├── context.md           # 收集的上下文信息
├── research.md          # 调查结果(代码库、技术验证等)
└── prd.md               # 最终 PRD(产品需求文档)
```

**特点**
- 最多 10 次迭代的对话会话
- 利用代码库调查和网络搜索进行技术验证
- 逐步具体化需求
- 考虑可实现性创建 PRD

---

### 2. sequence-diagram-analysis

**用途**
以 mermaid 格式创建指定功能的序列图。

**概述**
可视化功能处理流程,明确组件间的交互。分析代码库,图示函数调用、数据流和外部系统的协作。

**使用时机**
- 想理解复杂的处理流程时
- 想可视化现有功能的行为时
- 在重构或调试前掌握流程时
- 为创建文档或设计审查

**主要输出文件**
```
.dcs/{{timestamp}}_{{feature_name}}/
├── index.md              # 分析信息索引
├── tmp/                  # 临时调查结果(用于减少上下文)
│   ├── initial_survey.md
│   └── detailed_survey.md
└── sequence_diagram.md   # Mermaid 序列图和流程说明
```

**特点**
- 利用临时文件减少上下文
- 支持多种场景(正常流程·异常流程)
- 可调整详细级别(概述·详细·实现级别)
- 以 Mermaid 格式输出

---

### 3. state-transition-analysis

**用途**
识别与目标数据状态转换相关的所有功能,提供状态转换流程、相关数据的依赖关系和风险评估。

**概述**
全面分析实体或资源的状态管理,创建状态转换图、转换表、功能别分析、数据依赖关系和风险评估。

**使用时机**
- 想理解状态管理逻辑时
- 想可视化工作流或审批流程时
- 想列出与数据状态变更相关的功能时
- 想评估状态转换的一致性和风险时

**主要输出文件**
```
.dcs/{{timestamp}}_{{data_name}}/
├── index.md              # 分析信息索引
├── summary.md            # 分析摘要
├── diagram.md            # Mermaid 状态转换图
├── transitions.md        # 状态转换表
├── functions.md          # 功能别分析
├── dependencies.md       # 数据依赖关系分析
├── risks.md             # 风险评估
└── recommendations.md    # 建议事项
```

**特点**
- 自动检测状态字段
- 生成 Mermaid 状态转换图
- 全面的功能识别
- 相关数据的依赖关系分析
- 风险评估和建议事项

---

### 4. impact-analysis

**用途**
分析变更目标的影响范围,提供需要修改的文件和风险评估。

**概述**
对于文件、函数、类或以自然语言指定的变更目标,识别所有受影响的代码位置,并按层次整理。还提供风险评估和建议事项。

**使用时机**
- 代码变更前想把握影响范围时
- 想评估重构的影响时
- 想调查 API 变更或模型变更的影响时
- 制定审查或测试计划时

**主要输出文件**
```
.dcs/{{timestamp}}_{{target_name}}/
├── index.md              # 分析信息索引
├── summary.md            # 整体摘要
├── core.md              # 核心层的影响
├── api.md               # API 层的影响
├── service.md           # 服务层的影响
├── data.md              # 数据层的影响
├── ui.md                # UI 层的影响
├── test.md              # 对测试的影响
├── config.md            # 对配置文件的影响
├── other.md             # 其他影响
├── external.md          # 对外部系统的影响
├── recommendations.md    # 建议事项
└── details.md           # 详细信息
```

**特点**
- 按层次分析影响范围
- 评估风险级别
- 修改的优先级排序
- 测试策略建议

---

### 5. incremental-dev

**用途**
制定增量开发计划,为每种实现方法创建 PRD(产品需求文档)。

**概述**
分析开发对象,从初始调查到追加调查,提出多种实现方法。为每种方法创建详细的 PRD,制定阶段性实现计划。

**使用时机**
- 想制定新功能的实现计划时
- 想比较检讨多种实现方法时
- 想创建阶段性发布计划时
- 规划向现有系统添加功能时

**主要输出文件**
```
.dcs/{{timestamp}}_{{feature_name}}/
├── index.md                    # 分析信息索引
├── survey_summary.md           # 初始调查摘要
├── existing_impl.md            # 现有实现调查
├── tech_stack.md              # 技术栈
├── dependencies.md            # 依赖关系分析
├── complexity.md              # 复杂度评估
├── constraints.md             # 课题和约束
├── additional_research/       # 追加调查结果
│   └── {{research_topic}}.md
└── approaches/                # 每种实现方法的 PRD
    ├── approach_1_{{name}}.md
    ├── approach_2_{{name}}.md
    └── approach_3_{{name}}.md
```

**特点**
- 初始调查的全面信息收集
- 提出多种实现方法
- 创建每种方法的详细 PRD
- 阶段性实现计划
- 500 行以内的文件分割

---

### 6. bug-analysis

**用途**
实施详细分析以识别 Bug 的原因。

**概述**
从 Bug 的发生经过和症状识别原因,提出修复方针。详细分析源代码,逐步缩小原因范围。经过初始调查、处理流程分析、根本原因分析,创建最终报告。

**使用时机**
- Bug 原因不明时
- 想调查复杂的故障时
- 想分析再现性低的 Bug 时
- 想识别根本原因制定永久对策时

**主要输出文件**
```
.dcs/{{timestamp}}_{{bug_name}}/
├── index.md                    # 分析信息索引
├── initial_investigation.md    # 初始调查结果
├── flow_analysis.md           # 处理流程分析
├── root_cause_analysis.md     # 根本原因分析
└── final_report.md            # 最终报告
```

**特点**
- 交互式信息收集
- 逐步缩小原因范围
- 详细的处理流程分析
- 识别根本原因
- 提出修复方针

---

### 7. performance-analysis

**用途**
调查性能问题的原因。

**概述**
实施全面调查以识别性能问题的原因。逐步创建初始调查、详细原因分析和最终摘要,明确性能问题的根本原因。

**使用时机**
- 性能下降时
- 想调查响应慢的功能时
- 想识别可扩展性问题时
- 想确定优化的优先级时

**主要输出文件**
```
.dcs/{{timestamp}}_{{target_name}}/
├── index.md                # 分析信息索引
├── initial.md             # 初始调查结果
├── causes.md              # 原因分析
└── recommendations.md      # 建议对应
```

**特点**
- 通过 TodoWrite 进行任务管理
- 详细询问症状和发生情况
- 识别多个原因候选
- 有优先级的建议对应
- 识别性能瓶颈

---

## 共通的输出结构

所有命令都在 `.dcs/` 目录下创建带时间戳的目录,以如下结构输出结果:

```
.dcs/
└── {{timestamp}}_{{target_name}}/
    ├── index.md          # 分析概要和元信息
    ├── (各种分析结果文件)
    └── (子目录)
```

### 时间戳格式
`YYYYMMDD_HHMMSS` 格式(例: `20251030_143022`)

### 防止重复
如果存在相同时间戳·相同内容的目录,会在末尾添加序号。

---

## 可用工具

各命令可用的工具记载在文件头部的 `allowed-tools` 中:

- **Read, Glob, Grep**: 文件搜索·读取
- **Task**: 执行子任务(减少上下文)
- **Bash**: 执行 Shell 命令
- **AskUserQuestion**: 交互式信息收集
- **Write**: 文件写入
- **TodoWrite**: 任务管理(在 performance-analysis 中使用)
- **WebSearch, WebFetch**: Web 搜索·信息获取(在 feature-rubber-duck 中使用)

---

## 执行示例

### 执行 Bug 分析
```
/tsumiki:dcs:bug-analysis 购物车内的商品总金额计算不正确
```

### 执行影响范围分析
```
/tsumiki:dcs:impact-analysis src/models/User.ts
```

### 执行序列图创建
```
/tsumiki:dcs:sequence-diagram-analysis 订单确认处理
```

### 执行增量开发计划
```
/tsumiki:dcs:incremental-dev 添加用户认证功能
```

---

## 注意事项

1. **交互型命令**: 所有命令都以交互方式收集信息。通过回答问题可以进行更准确的分析。

2. **输出目录**: 所有输出都汇总到 `.dcs/` 目录中。建议添加到 gitignore。