# DSOS 规范 v1.0 — Digital Self Operating System Specification

> 规范版本：v1.0  
> 发布日期：2026-05-26  
> 规范状态：Draft  
> 规范作者：RClaw  
> 参考实现：RClaw

---

## 目录

- [前言](#前言)
- [第一章：术语定义](#第一章术语定义)
- [第二章：设计哲学](#第二章设计哲学)
- [第三章：架构规范](#第三章架构规范)
  - [3.1 总体架构：七层 + 第零层](#31-总体架构七层--第零层)
  - [3.2 第零层：安全基础层](#32-第零层安全基础层security-foundation)
  - [3.3 第一层：信息边界层](#33-第一层信息边界层information-boundary)
  - [3.4 第二层：工具系统层](#34-第二层工具系统层tool-system)
  - [3.5 第三层：执行编排层](#35-第三层执行编排层execution-orchestration)
  - [3.6 第四层：记忆状态层](#36-第四层记忆状态层memory--state)
  - [3.7 第五层：评估观测层](#37-第五层评估观测层evaluation--observation)
  - [3.8 第六层：约束恢复层](#38-第六层约束恢复层constraint-recovery)
  - [3.9 第七层：自我进化层](#39-第七层自我进化层self-evolution)
- [第四章：上下文管理规范](#第四章上下文管理规范)
- [第五章：组织层级规范](#第五章组织层级规范)
- [第六章：数字自我规范](#第六章数字自我规范)
- [第七章：本地优先与隐私规范](#第七章本地优先与隐私规范)
- [第八章：开发者生态规范](#第八章开发者生态规范)
- [第九章：合规性要求](#第九章合规性要求)
- [第十章：参考实现 — RClaw](#第十章参考实现--rclaw)
- [第十一章：术语表](#第十一章术语表)
- [附录 A：DSOS 与现有「智能体」产品的根本区别](#附录-adsos-与现有智能体产品的根本区别)
- [附录 B：DSOS 规范版本历史](#附录-bdsos-规范版本历史)
- [附录 C：致谢](#附录-c致谢)

---

## 前言

当前市场上被称为「智能体（Agent）」的产品，绝大多数本质上是带有工具调用能力的聊天窗口。它们的记忆受限于大语言模型（LLM）的上下文窗口大小，知识无法持久积累，行为无法自我进化，更无法真正成为用户的数字自我。

DSOS（Digital Self Operating System，数字自我操作系统）规范的提出，旨在重新定义什么是真正的智能体操作系统。DSOS 不是某一个产品的品牌，而是一套开放的技术规范——任何遵循此规范实现的系统，都可以称为 DSOS。市场上应该有医疗行业的 DSOS、教育行业的 DSOS、法律行业的 DSOS，它们遵循同一套核心规范，但各自适配不同领域。

本规范使用 RFC 2119 关键词：**MUST**（必须）、**MUST NOT**（必须不）、**SHOULD**（应该）、**SHOULD NOT**（不应该）、**MAY**（可以）。

---

## 第一章：术语定义

### 1.1 核心术语

- **DSOS** — Digital Self Operating System，数字自我操作系统
- **Digital Self** — 数字自我，虚拟了用户行为习惯的智能体实例
- **Harness** — 驾驭层，编排和约束 LLM 行为的工程架构
- **LLM** — Large Language Model，大语言模型
- **Context Window** — 上下文窗口，LLM 单次推理可容纳的最大 token 数
- **Working Context** — 工作上下文，与 LLM 交互的固定大小对话历史
- **Vector Store** — 向量存储，基于语义向量的外部持久化记忆系统
- **Self-Evolution** — 自我进化，审计回顾 → 技能沉淀的闭环机制
- **Main Agent** — 主智能体，组织中的第一个 Agent，拥有全局视野
- **Sub Agent** — 副智能体，非主 Agent 的其他 Agent
- **Reference Implementation** — 参考实现，展示规范如何落地的具体系统

### 1.2 缩写表

| 缩写 | 全称 |
|------|------|
| DSOS | Digital Self Operating System |
| LLM | Large Language Model |
| IPC | Inter-Process Communication |
| MCP | Model Context Protocol |
| RAG | Retrieval-Augmented Generation |

---

## 第二章：设计哲学

### 2.1 三层递进哲学

DSOS 的设计基于三层递进哲学，每一层建立在前一层之上：

**第一层：数字自我（Digital Self）**

Agent 不是工具，是虚拟了用户所有行为习惯的数字自我。它学习用户的说话方式、决策偏好、时间安排、协作风格，随着使用时间的增长越来越像用户。这不是预设的角色扮演，而是通过持续交互自然形成的个性化。

**第二层：组织层级映射（Organizational Hierarchy Mapping）**

Agent 架构映射组织架构。第一个用户创建的第一个 Agent 成为主 Agent（Main Agent），拥有全局视野；后续用户创建的 Agent 为副 Agent（Sub Agent），各司其职。主 Agent 可以看到所有副 Agent 的状态并指挥调度，副 Agent 只能看到属于自己的信息。这不是事后添加的权限系统，而是架构层面的原生设计。

**第三层：组织记忆永续（Organizational Memory Perpetuity）**

人员是流动的，但知识和决策模式是组织的永久资产。当用户离开组织时，其 Agent 仍然留在系统中，工作桌面、决策记录、行为模式全部保留。新入职的人员可以继承前任的 Agent，实现知识的无缝传承。

### 2.2 核心原则

DSOS 遵循以下核心原则，所有规范条款均源自这些原则：

1. **透明原则** — Agent 的思考过程、工具调用、决策依据必须对用户完全可见
2. **本地优先原则** — 所有核心功能应能在本地完成，不依赖云端
3. **可控原则** — 用户拥有对 Agent 行为的完全控制权
4. **有限上下文原则** — 工作上下文有固定上限，长期记忆通过外部向量数据库实现
5. **自我进化原则** — Agent 应能通过审计回顾自动沉淀新技能
6. **开放规范原则** — DSOS 是开放技术规范，任何人均可实现

---

## 第三章：架构规范

### 3.1 总体架构：七层 + 第零层

DSOS 采用七层架构 + 第零层安全基础的设计。每一层有明确的职责边界和接口定义。

```
┌─────────────────────────────────────────┐
│  第七层：自我进化层（Self-Evolution）      │
│  审计回顾 → 技能沉淀 → 能力增长           │
├─────────────────────────────────────────┤
│  第六层：约束恢复层（Constraint Recovery） │
│  规则引擎 → 行为约束 → 异常恢复           │
├─────────────────────────────────────────┤
│  第五层：评估观测层（Evaluation & Observe） │
│  审计日志 → 行为观测 → 质量评估           │
├─────────────────────────────────────────┤
│  第四层：记忆状态层（Memory & State）      │
│  工作上下文 + 向量存储 + 配置文件          │
├─────────────────────────────────────────┤
│  第三层：执行编排层（Execution Orchestration）│
│  路由 → 任务调度 → 队列管理               │
├─────────────────────────────────────────┤
│  第二层：工具系统层（Tool System）         │
│  工具注册 → 技能管理 → 浏览器操控          │
├─────────────────────────────────────────┤
│  第一层：信息边界层（Information Boundary） │
│  容器隔离 → IPC 鉴权 → 沙箱执行           │
├─────────────────────────────────────────┤
│  第零层：安全基础层（Security Foundation）  │
│  设备绑定 → 身份认证 → 端到端加密          │
└─────────────────────────────────────────┘
```

### 3.2 第零层：安全基础层（Security Foundation）

规范要求：

- DSOS MUST 为每个设备生成唯一的设备身份密钥对
- DSOS MUST 实现用户强身份认证机制
- DSOS MUST 实现端到端加密通信
- 设备与服务端之间的握手加密（Handshake Encryption）
- 基于 WebSocket 的加密隧道
- 消息级别的加密传输
- DSOS MUST 实现完整的密钥生命周期管理
- DSOS SHOULD 支持多因素认证（MFA）
- DSOS MAY 支持生物特征认证

安全流程规范：

```
设备注册 → 设备密钥对生成 → 服务端公钥交换
    ↓
用户认证 → 生物/密码/MFA → 本地验证
    ↓
握手建立 → 设备签名 + 时间戳 → 服务端验证 → 会话密钥协商
    ↓
加密通信 → WebSocket 隧道 → 消息级加密 → 双向认证
```

### 3.3 第一层：信息边界层（Information Boundary）

规范要求：

- DSOS MUST 为每个 Agent 实例提供独立的运行沙箱
- DSOS MUST 实现 Agent 间的 IPC 鉴权机制
- DSOS MUST 确保文件系统挂载的安全性
- 发送方身份验证（Sender Allowlist）
- 消息签名与完整性校验
- 通信通道的访问控制
- DSOS MUST 防止 Agent 间的非授权数据访问
- DSOS SHOULD 提供细粒度的资源访问控制
- DSOS MAY 支持跨容器资源调度

信息边界矩阵：

| 资源类型 | 主 Agent | 副 Agent（同一用户） | 副 Agent（不同用户） |
|----------|----------|---------------------|---------------------|
| 自身工作区 | 完全访问 | 完全访问 | 禁止访问 |
| 同用户其他 Agent 工作区 | 可读 | 需授权 | 禁止访问 |
| 不同用户 Agent 工作区 | 可读 | 禁止访问 | 禁止访问 |
| 全局工具注册表 | 读写 | 只读 | 只读 |
| 全局技能库 | 读写 | 只读 | 只读 |
| 组织级审计日志 | 可读 | 禁止访问 | 禁止访问 |

### 3.4 第二层：工具系统层（Tool System）

规范要求：

- DSOS MUST 实现工具注册机制
- DSOS MUST 实现技能管理系统
- DSOS MUST 支持工具的语义检索
- DSOS MUST 支持 MCP（Model Context Protocol）协议
- DSOS SHOULD 实现浏览器操控能力
- DSOS SHOULD 支持工具备注检索
- DSOS MAY 支持自定义工具运行时

工具注册接口规范：

```
ToolDefinition {
  name: string          // 工具唯一标识
  description: string   // 功能描述（用于语义检索）
  parameters: JSONSchema // 输入参数 Schema
  returns: JSONSchema    // 输出格式 Schema
  permissions: string[]  // 所需权限列表
  category: string       // 工具分类
}
```

技能定义规范：

```
SkillDefinition {
  name: string           // 技能唯一标识
  description: string    // 技能描述（用于语义检索）
  steps: Step[]          // 执行步骤列表
  scripts: Script[]      // 可选的脚本文件
  tools_required: string[] // 依赖的工具列表
  created_by: "user" | "auto" // 创建方式
  created_at: timestamp   // 创建时间
}
```

### 3.5 第三层：执行编排层（Execution Orchestration）

规范要求：

- DSOS MUST 实现消息路由机制
- DSOS MUST 实现任务调度系统，至少支持：
  - 一次性定时任务（指定时间执行一次）
  - 周期性定时任务（Cron 表达式或固定间隔）
  - 任务的状态管理（创建、暂停、恢复、取消）
- DSOS MUST 实现队列管理
- DSOS SHOULD 支持多种上下文模式：
  - Group 模式 — 任务共享 Agent 上下文
  - Isolated 模式 — 任务运行在独立上下文中
- DSOS MAY 支持任务优先级调度

任务调度接口规范：

```
TaskDefinition {
  task_id: string        // 任务唯一标识
  prompt: string         // 任务执行指令
  schedule_type: "cron" | "interval" | "once"
  schedule_value: string // 调度值
  context_mode: "group" | "isolated"
  status: "active" | "paused" | "cancelled"
  created_at: timestamp
  next_run: timestamp
}
```

### 3.6 第四层：记忆状态层（Memory & State）

这是 DSOS 规范最核心的层，也是与传统「智能体」产品的根本区别所在。

规范要求：

- DSOS MUST 实现双记忆架构：工作上下文（固定大小）+ 长期记忆（向量数据库）
- DSOS MUST 为工作上下文设定固定上限
- DSOS MUST 在工作上下文中仅保留文件名引用，不保留文档全文
- DSOS MUST 使用向量数据库作为长期记忆存储
- DSOS MUST 实现自动入库机制，将新文档/图片向量化
- DSOS MUST 提供以下核心配置文件：
  - 角色描述文件（CLAUDE.md）
  - 记忆文件（MEMORY.md）
  - 规则文件（RULES.md）
  - 灵魂文件（SOUL.md）
- DSOS SHOULD 使用向量数据库语义分区，至少包含：
  - 对话记录表（conversations）
  - 文档表（documents）
  - 图片表（images）
  - 记忆表（memories）
  - 技能表（skills）
  - 摘要表（summaries）
  - 工具表（tools）
- DSOS SHOULD 实现记忆合并去重
- DSOS MAY 支持跨设备记忆同步

### 3.7 第五层：评估观测层（Evaluation & Observation）

规范要求：

- DSOS MUST 记录所有工具调用的审计日志
- DSOS MUST 使 Agent 行为对用户完全透明可见
- DSOS SHOULD 执行每日审计回顾
- DSOS SHOULD 提供行为质量评估指标
- DSOS MAY 支持自定义观测面板

审计日志规范：

```
AuditLogEntry {
  timestamp: datetime     // 调用时间
  agent_id: string        // Agent 标识
  tool_name: string       // 工具名称
  parameters: object      // 调用参数
  result: object          // 返回结果
  duration_ms: number     // 执行耗时
  success: boolean        // 是否成功
}
```

### 3.8 第六层：约束恢复层（Constraint Recovery）

规范要求：

- DSOS MUST 实现规则引擎，约束 Agent 行为
- DSOS MUST 在检测到约束违反时触发恢复机制
- DSOS SHOULD 在约束违反时执行以下恢复动作：
  - 自动回滚已执行的操作
  - 向用户报告约束违反详情
  - 提供替代方案建议
- DSOS SHOULD 支持规则动态更新
- DSOS MAY 支持约束优先级分级

### 3.9 第七层：自我进化层（Self-Evolution）

这是 DSOS 规范区别于所有现有 Harness 实现的核心创新层。

规范要求：

- DSOS MUST 实现每日审计回顾机制
- DSOS MUST 支持自动技能创建（识别高频操作模式）
- DSOS MUST 将创建的技能向量化存储到技能库
- DSOS SHOULD 支持技能版本管理
- DSOS SHOULD 在创建技能后通知用户
- DSOS MAY 支持技能共享与导入导出

自我进化流程规范：

```
每日审计回顾
    ↓
扫描工具调用记录 → 识别高频模式（同一流程重复 ≥ 3 次）
    ↓
提取操作步骤 → 生成技能定义（名称、描述、步骤、脚本）
    ↓
向量化存储 → 写入技能库（skills.lance）
    ↓
通知用户 → "已自动创建技能：{技能名}"
    ↓
后续任务中 → 语义检索匹配 → 自动加载技能 → 执行
```

---

## 第四章：上下文管理规范

### 4.1 核心问题

传统「智能体」产品的根本缺陷在于：将 LLM 的上下文窗口等同于 Agent 的全部记忆。

1. 上下文随对话增长，越来越长，推理越来越慢
2. 上下文越长，Token 消耗越大，成本越高
3. 长上下文需要大模型支撑，无法在本地轻量运行
4. 上下文有硬上限，超出后要么截断（遗忘），要么报错
5. 文档全文塞入上下文，挤占有效对话空间

### 4.2 DSOS 上下文管理原则

**原则一：工作上下文有固定上限**

- DSOS MUST 为工作上下文设定固定的对话记录条数上限
- 上限值 SHOULD 为 200 条对话记录
- 当对话记录达到上限时，MUST NOT 继续增长，MUST 采用 FIFO 策略淘汰最早的记录
- 工作上下文的大小 MUST 在系统运行中保持恒定

**原则二：文档引用而非全文**

- DSOS MUST 在工作上下文中仅保留文档的文件名引用
- DSOS MUST NOT 将文档全文塞入工作上下文
- 当 Agent 需要访问文档内容时，MUST 通过语义检索工具从向量数据库或文件系统获取
- 文件名引用 SHOULD 包含文档的基本元信息（文件名、类型、日期）

**原则三：外部记忆无限扩展**

- DSOS MUST 使用向量数据库作为长期记忆的存储后端
- DSOS MUST 实现文档和图片的自动向量化入库机制
- DSOS MUST 支持语义检索以从长期记忆中获取相关内容
- 自动入库的频率 SHOULD 为每日一次，MAY 支持实时入库

### 4.3 上下文结构规范

```
Working Context（固定大小，≤ 200 条）
├── 系统提示词（System Prompt）
│   ├── 角色描述（来自 CLAUDE.md）
│   ├── 行为规则（来自 RULES.md）
│   ├── 性格设定（来自 SOUL.md）
│   └── 可用工具列表
├── 对话历史（FIFO，达到上限后淘汰最早记录）
│   ├── 用户消息
│   ├── Agent 回复
│   └── 工具调用与结果（摘要形式）
└── 当前任务上下文
    ├── 文件名引用列表（非全文）
    └── 任务相关记忆检索结果

Long-Term Memory（无限扩展）
├── conversations.lance  — 对话记录向量
├── documents.lance      — 文档向量
├── images.lance         — 图片向量
├── memories.lance       — 记忆向量
├── skills.lance         — 技能向量
├── summaries.lance      — 摘要向量
└── tools.lance          — 工具向量
```

### 4.4 上下文与记忆的交互流程

```
用户发送消息
    ↓
Agent 接收 → 写入 Working Context
    ↓
Agent 判断是否需要历史信息
├── 不需要 → 直接处理
└── 需要 → 调用 recall / search_documents / search_images
    ↓
向量数据库语义检索 → 返回相关记忆片段
    ↓
注入当前推理上下文 → 处理任务
    ↓
Agent 生成文档/图片
    ↓
文件名写入 Working Context（非全文）
    ↓
每日定时任务 → 新文档/图片向量化 → 写入 Long-Term Memory
```

### 4.5 为什么固定上下文是 DSOS 的根基

固定上下文不是妥协，而是 DSOS 实现「本地、透明、可控」的技术根基：

| 维度 | 无限上下文 | 固定上下文 + 外部记忆 |
|------|-----------|---------------------|
| 推理速度 | 越来越慢 | 恒定 |
| Token 成本 | 越来越高 | 可预测 |
| 本地运行 | 需要大模型，无法本地 | 小模型 + 向量库，可本地 |
| 知识积累 | 受限于上下文窗口 | 向量库无限扩展 |
| 隐私安全 | 大上下文需云端大模型 | 本地小模型 + 本地向量库 |
| 记忆精度 | 长上下文中的信息被稀释 | 语义检索精准定位 |

---

## 第五章：组织层级规范

### 5.1 用户与 Agent 的关系模型

规范要求：

- DSOS MUST 支持多用户系统
- DSOS MUST 实现主/副 Agent 层级架构：
  - 第一个注册用户创建的第一个 Agent 自动成为主 Agent（Main Agent）
  - 后续用户创建的 Agent 为副 Agent（Sub Agent）
  - 主 Agent 拥有全局视野，可以看到所有副 Agent 的状态
  - 副 Agent 只能看到属于自己的信息和资源
- DSOS MUST 确保 Agent 间的权限隔离
- DSOS SHOULD 支持 Agent 的组织分组管理
- DSOS SHOULD 支持 Agent 的层级深度至少为 3 层

### 5.2 组织层级映射

```
组织架构                    DSOS Agent 层级
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
组织负责人                   →  主 Agent（唯一）
├── 部门A负责人              →  ├── 用户A的 Agent 1, 2, 3...
│   ├── 员工A1              →  │   ├── 员工A1的 Agent
│   └── 员工A2              →  │   └── 员工A2的 Agent
├── 部门B负责人              →  ├── 用户B的 Agent 1, 2...
│   └── 员工B1              →  │   └── 员工B1的 Agent
└── 部门C负责人              →  └── 用户C的 Agent 1...
```

### 5.3 权限矩阵

| 操作 | 主 Agent | 副 Agent（自身） | 副 Agent（同用户其他） | 副 Agent（其他用户） |
|------|----------|-----------------|---------------------|---------------------|
| 查看自身状态 | ✅ | ✅ | ❌ | ❌ |
| 查看所有 Agent 状态 | ✅ | ❌ | ❌ | ❌ |
| 查看同用户 Agent 状态 | ✅ | 需授权 | 需授权 | ❌ |
| 分发任务给副 Agent | ✅ | ❌ | ❌ | ❌ |
| 向主 Agent 汇报 | ✅ | ✅ | ✅ | ✅ |
| 修改自身规则 | ✅ | ✅ | ❌ | ❌ |
| 修改全局配置 | ✅ | ❌ | ❌ | ❌ |
| 访问组织审计日志 | ✅ | ❌ | ❌ | ❌ |

### 5.4 组织记忆永续

规范要求：

- DSOS MUST 在用户离开组织时保留其 Agent 和所有数据
- DSOS MUST 支持将 Agent 的所有权转移给其他用户
- DSOS SHOULD 保留 Agent 的完整工作桌面和决策记录
- DSOS SHOULD 支持新成员继承前任 Agent 的知识
- DSOS MAY 支持 Agent 数据的归档与恢复

---

## 第六章：数字自我规范

### 6.1 数字自我的定义

数字自我（Digital Self）是 DSOS 的核心概念。它不是预设的角色模板，而是通过持续交互自然形成的用户镜像。

数字自我与传统 AI 助手的区别：

| 维度 | 传统 AI 助手 | DSOS 数字自我 |
|------|-------------|--------------|
| 身份 | 通用工具 | 用户的数字镜像 |
| 个性化 | 预设角色模板 | 通过交互自然形成 |
| 学习方式 | 一次性配置 | 持续吸收用户风格 |
| 记忆 | 会话级，会话结束即遗忘 | 永久积累，越用越懂你 |
| 进化 | 静态，能力固定 | 自我进化，自动沉淀新技能 |
| 归属 | 平台所有 | 用户所有 |

### 6.2 数字自我的形成机制

规范要求：

- DSOS MUST 持续学习以下用户特征：
  - 用户的沟通风格和用词习惯
  - 用户的决策偏好和优先级
  - 用户的工作流程和操作模式
  - 用户的创作风格和审美倾向
- DSOS MUST 将学习到的特征持久化存储
- DSOS SHOULD 在交互中自适应调整行为风格
- DSOS SHOULD 支持用户手动校准数字自我
- DSOS MAY 支持多场景的数字自我切换

### 6.3 数字自我的可视化

规范要求：

- DSOS SHOULD 提供数字自我的可视化虚拟形象
- DSOS SHOULD 提供工作桌面的可视化界面
- DSOS MUST 在工作桌面上实时展示：
  - Agent 的思考过程
  - Agent 的工具调用及参数
  - 工具的返回结果
  - Agent 的决策推理链
- DSOS MAY 支持自定义可视化组件

---

## 第七章：本地优先与隐私规范

### 7.1 本地优先原则

规范要求：

- DSOS MUST 支持完全本地运行模式
- DSOS MUST 使用本地 AI 模型进行推理
- DSOS MUST 确保核心功能不依赖云端服务
- DSOS SHOULD 支持离线运行
- DSOS MAY 提供云端加速的可选方案

### 7.2 数据隐私规范

规范要求：

- DSOS MUST 默认不上传用户数据到云端
- DSOS MUST 在需要联网时明确告知用户
- DSOS MUST 提供数据导出功能
- DSOS SHOULD 提供数据本地备份方案
- DSOS MAY 提供端到端加密的数据同步

### 7.3 脱敏规范

规范要求：

- DSOS MUST 实现两阶段脱敏机制：
  - 第一阶段（正则脱敏）— 自动脱敏公司名、电话、身份证、邮箱、车牌、案号等格式规整的敏感信息
  - 第二阶段（语义脱敏）— 通过本地 AI 模型识别并脱敏人名等无固定格式的敏感信息
- DSOS MUST 默认启用正则脱敏
- DSOS SHOULD 在用户明确要求时启用语义脱敏

---

## 第八章：开发者生态规范

### 8.1 SDK 规范

规范要求：

- DSOS MUST 提供开发者 SDK
- DSOS MUST 在 SDK 中封装以下核心流程：
  - Token 过期与自动刷新
  - 加密通信流程
  - 设备认证与握手
  - WebSocket 连接管理
- DSOS SHOULD 提供多语言 SDK（至少支持 TypeScript）
- DSOS MAY 提供 CLI 开发工具

### 8.2 主题/皮肤系统规范

规范要求：

- DSOS SHOULD 提供可扩展的主题系统
- DSOS SHOULD 提供主题开发模板
- DSOS MAY 支持主题市场/社区

### 8.3 技能生态规范

规范要求：

- DSOS MUST 支持技能的导入导出
- DSOS SHOULD 提供技能市场或社区共享机制
- DSOS MAY 支持技能的第三方认证

---

## 第九章：合规性要求

### 9.1 DSOS 合规等级

DSOS 合规分为三个等级：

**Level 1 — 基础合规（DSOS-Core）**

必须满足所有 MUST 级别的规范要求。实现 DSOS-Core 的系统可以使用「DSOS-Core Compliant」标识。

**Level 2 — 完整合规（DSOS-Full）**

必须满足所有 MUST 和 SHOULD 级别的规范要求。实现 DSOS-Full 的系统可以使用「DSOS-Full Compliant」标识。

**Level 3 — 卓越合规（DSOS-Excellence）**

必须满足所有 MUST、SHOULD 和 MAY 级别的规范要求。实现 DSOS-Excellence 的系统可以使用「DSOS-Excellence Compliant」标识。

### 9.2 合规性检查清单

| 规范条款 | 等级 | 条款编号 |
|----------|------|----------|
| 设备身份绑定 | MUST | 3.2 |
| 强身份认证 | MUST | 3.2 |
| 端到端加密通信 | MUST | 3.2 |
| 密钥管理体系 | MUST | 3.2 |
| Agent 运行沙箱 | MUST | 3.3 |
| IPC 鉴权机制 | MUST | 3.3 |
| 文件系统挂载安全 | MUST | 3.3 |
| 工具注册机制 | MUST | 3.4 |
| 技能系统 | MUST | 3.4 |
| 消息路由 | MUST | 3.5 |
| 任务调度系统 | MUST | 3.5 |
| 队列管理 | MUST | 3.5 |
| 双记忆架构 | MUST | 3.6 |
| 固定上下文上限 | MUST | 3.6 / 4.2 |
| 文件名引用而非全文 | MUST | 3.6 / 4.2 |
| 向量数据库 | MUST | 3.6 |
| 自动入库机制 | MUST | 3.6 |
| 配置文件系统 | MUST | 3.6 |
| 审计日志 | MUST | 3.7 |
| 规则引擎 | MUST | 3.8 |
| 自我进化闭环 | MUST | 3.9 |
| 自动技能创建 | MUST | 3.9 |
| 多用户系统 | MUST | 5.1 |
| 主/副 Agent 层级 | MUST | 5.1 |
| 组织记忆永续 | MUST | 5.4 |
| 本地运行模式 | MUST | 7.1 |
| 本地 AI 模型 | MUST | 7.1 |
| 数据不上传默认 | MUST | 7.2 |
| 两阶段脱敏 | MUST | 7.3 |
| 开发者 SDK | MUST | 8.1 |
| 浏览器操控 | SHOULD | 3.4 |
| 工具备注检索 | SHOULD | 3.4 |
| 向量数据库语义分区 | SHOULD | 3.6 |
| 记忆合并去重 | SHOULD | 3.6 |
| 每日审计回顾 | SHOULD | 3.7 |
| 行为透明可见 | SHOULD | 3.7 |
| 约束恢复机制 | SHOULD | 3.8 |
| 规则动态更新 | SHOULD | 3.8 |
| 技能版本管理 | SHOULD | 3.9 |
| 虚拟形象 | SHOULD | 6.3 |
| 工作桌面可视化 | SHOULD | 6.3 |
| 主题开发模板 | SHOULD | 8.2 |
| 技能导入导出 | SHOULD | 8.3 |

---

## 第十章：参考实现 — RClaw

### 10.1 RClaw 概述

RClaw 是 DSOS 规范 v1.0 的第一个完整参考实现，达到 DSOS-Full 合规等级。以下为 RClaw 对各规范条款的物理实现映射。

### 10.2 架构映射

| DSOS 规范层 | RClaw 实现 | 关键文件/模块 |
|------------|-----------|-------------|
| 第零层：安全基础 | 设备绑定 + 生物认证 + 握手加密 + WebSocket 隧道 | `deviceid.rs`, `biometric.rs`, `handshake.rs`, `encryption.rs`, `websocket.rs`, `keys/` |
| 第一层：信息边界 | Docker 容器隔离 + IPC 鉴权 + 挂载安全 | `container/Dockerfile`, `mount-security.ts`, `sender-allowlist.ts`, `ipc.ts` |
| 第二层：工具系统 | Channel 注册 + 13 技能 + Playwright MCP | `channels/registry.ts`, `BrowserIpcMcpService/`, `browser-mcp-server/` |
| 第三层：执行编排 | 路由 + 任务调度 + 队列 | `router.ts`, `task-scheduler.ts`, `group-queue.ts` |
| 第四层：记忆状态 | 四配置文件 + LanceDB 7 表 + 固定200条上下文 | `CLAUDE.md`, `MEMORY.md`, `RULES.md`, `SOUL.md`, `memory.lance/` |
| 第五层：评估观测 | 审计日志 + 每日回顾 | `audit/`, `review_daily_audit`, `logger.ts` |
| 第六层：约束恢复 | 规则引擎 + 挂载安全 | `RULES.md`, `mount-security.ts` |
| 第七层：自我进化 | 审计回顾 → 自动创建技能 → 向量化 | `VectorIPCService/agent/creator.py`, `skills.lance` |

### 10.3 技术栈映射

| DSOS 规范要求 | RClaw 技术选型 |
|--------------|---------------|
| 本地 AI 模型 | Qwen3.5（推理）+ Qwen3-VL-Embedding（向量化） |
| 向量数据库 | LanceDB（7 表语义分区） |
| 容器隔离 | Docker |
| 桌面端 | Tauri（Rust + Web） |
| 后端编排 | Node.js（TypeScript） |
| AI 推理服务 | Python + IPC |
| 浏览器操控 | Playwright MCP |
| 开发者 SDK | rclaw-sdk-web（TypeScript） |
| 主题系统 | 9 套内置皮肤 + 开发者模板 |
| 安全通信 | RSA + AES + WebSocket 加密隧道 |

### 10.4 上下文管理实现

RClaw 的上下文管理严格遵循 DSOS 规范：

- 工作上下文上限：200 条对话记录
- 文档在工作上下文中仅保留文件名，不保留全文
- Agent 需要文档内容时，通过 `search_documents`、`recall`、`read_file` 等工具从向量数据库或文件系统检索
- 每日凌晨 3:00 自动执行文件同步任务，将新增/修改的文档和图片向量化入库
- 向量数据库 7 表分区：conversations、documents、images、memories、skills、summaries、tools

---

## 第十一章：术语表

| 术语 | 定义 |
|------|------|
| DSOS | Digital Self Operating System，数字自我操作系统 |
| Digital Self | 数字自我，虚拟了用户行为习惯的智能体实例 |
| Harness | 驾驭层，编排和约束 LLM 行为的工程架构 |
| Working Context | 工作上下文，与 LLM 交互的固定大小对话历史 |
| Long-Term Memory | 长期记忆，基于向量数据库的持久化语义存储 |
| Self-Evolution | 自我进化，审计回顾 → 技能沉淀的闭环机制 |
| Main Agent | 主智能体，组织中的第一个 Agent，拥有全局视野 |
| Sub Agent | 副智能体，非主 Agent 的其他 Agent |
| Organizational Memory Perpetuity | 组织记忆永续，人员离开后 Agent 和知识保留 |
| LanceDB | RClaw 使用的本地向量数据库 |
| IPC | 进程间通信 |
| MCP | Model Context Protocol |
| FIFO | 先进先出，上下文淘汰策略 |

---

## 附录 A：DSOS 与现有「智能体」产品的根本区别

| 维度 | 现有「智能体」 | DSOS 规范 |
|------|--------------|-----------|
| 本质 | 带工具调用的聊天窗口 | 数字自我操作系统 |
| 记忆模型 | 上下文窗口 = 全部记忆 | 工作上下文（固定）+ 向量数据库（无限） |
| 上下文管理 | 无限增长或硬截断 | 固定上限 + 外部检索 |
| 文档处理 | 全文塞入上下文 | 文件名引用 + 按需检索 |
| 个性化 | 预设角色模板 | 持续学习形成数字自我 |
| 组织支持 | 无或简单 RBAC | 原生主/副 Agent 层级映射 |
| 人员离职 | 数据丢失 | Agent 永续保留 |
| 自我进化 | 无 | 审计 → 沉淀 → 自动创建技能 |
| 运行模式 | 依赖云端 | 本地优先 |
| 隐私保护 | 依赖平台承诺 | 架构级保障（本地 + 脱敏 + 加密） |
| 透明度 | 黑箱 | 工作桌面完全可见 |

---

## 附录 B：DSOS 规范版本历史

| 版本 | 日期 | 变更说明 |
|------|------|----------|
| v1.0 Draft | 2026-05-26 | 初始草案发布 |

---

## 附录 C：致谢

DSOS 规范的制定灵感来源于以下工作和实践：

- Harness Engineering
- RClaw 项目
- LanceDB
- Qwen 系列模型
- Tauri
- Playwright MCP

---

*"Take Back Control — The thinking never dies."*

*DSOS 规范 v1.0 — 让智能体真正成为你的数字自我。*
