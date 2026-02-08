# 📘 Aura Task Graph Protocol (TGP) - 核心执行标准 (v1.0)
<!-- 文件名: AUP-0001-TGP-CORE-SPEC.md -->

**核心原则：** 本协议定义了 Aura OS 生态内，智能体任务（Task Graph）的最小可执行结构，旨在保证跨 Agent/跨领域任务的**通用性、可追溯性与执行的可靠性**。

---

## 1. 协议元数据 (Protocol & Document Metadata)

| 字段 (Field) | 类型 (Type) | 约束/描述 (Constraint/Description) | 核心价值 |
| :--- | :--- | :--- | :--- |
| **`protocol_version`** | string | **v1.0** (不可变)。用于客户端和服务端进行版本校验。 | 协议的稳定性 |
| **`specification`** | string | **TASK_GRAPH_EXECUTION**。定义本文件是任务图执行规范。 | 协议的身份与通用性 |

---

## 2. 任务图声明体 (The Task Graph Manifest)

该部分定义了任务图（Task Graph）作为“数字资产”的身份和核心配置。

| 字段 (Field) | 类型 (对应你的代码) | 约束/描述 | 核心价值 |
| :--- | :--- | :--- | :--- |
| **`sop_id`** | UUID | **任务图的全局唯一 ID**。作为资产的身份证明。 | **资产所有权** |
| **`created_by`** | UUID | 指向创作者 (User ID)。 | **知识产权** |
| **`name`** | string | 任务图的用户友好名称 (e.g., "电网储能调度逻辑", "黄金趋势跟踪策略")。 | **前端展示与搜索** |
| **`description`** | string | 任务图的功能描述。 | **技能市场搜索** |
| **`dsl_version`** | string | 核心 DSL 语法版本，必须为 **"5.0"**。 | **技术兼容性** |
| **`runtime_context`** | Map<string, any> | **运行时上下文配置**。Agent 启动时注入的参数（如 `execution_account_id`, `risk_limit`, `physical_location`）。 | **安全与治理** |
| **`profile`** | AgentProfile 对象 | **数字员工档案**。用于定义 Agent 的组织归属（部门、团队、岗位）。 | **组织化管理** |

---

## 3. 核心执行结构 (The Execution Flow)

该部分定义了任务图的**拓扑骨架**和**步骤的原子定义**。

### 3.1 任务执行节点 (`NodeBlock` / **Vertex**)

这是构成任务图的最小执行单元，对应一个可调用的 **Skill/功能**。

| 字段 (Field) | 类型 | 约束/描述 |
| :--- | :--- | :--- |
| **`id`** | string | 节点在流程图中的唯一ID (e.g., `calc_ema`)。 |
| **`type`** | string | **执行功能 (Skill) 的唯一标识**。必须在 Aura OS 的 Manifest 注册表中存在（如 `kafka_source`, `position_manager`）。 |
| **`params`** | Map<string, any> | 传递给**执行功能**的配置参数。 |

### 3.2 流转定义 (`WiringBlock` / **Edge**)

定义了任务节点间的数据流和事件依赖。

| 字段 (Field) | 类型 | 约束/描述 |
| :--- | :--- | :--- |
| **`from`** | string | **源端口**：格式为 `[NodeID].[OutputPortName]` (如 `data_source.BarOutput`)。 |
| **`to`** | string | **目标端口**：格式为 `[NodeID].[InputPortName]` (如 `calc_ema.BarInput`)。 |
| **`connection_type`** | string | **流转类型**：`event` (事件驱动) 或 `data_stream` (数据流) 或 `control_flow` (控制流)。定义连接的底层机制。 |

---

## 4. 扩展与演进 (Extensions & Future Proofing)

*   **`extensions` 字段：** 任何任务图 Manifest **必须**包含一个顶层的 `extensions: Map<string, any>` 字段。
*   **用途：** 预留给未来未定义的特性，例如 **条件判断逻辑、并行执行参数、视觉流程图坐标**等。
*   **演进原则：** 任何新功能都必须先在 `extensions` 中进行试点，只有被社区广泛采用后，才能在 v2.0 中升级为标准字段。

---
