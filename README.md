# 🤖 Aura OS - Agent Development Kit (ADK)

**The Universal Execution and Governance Standard for AI-Native Agents.**

---

## 💡 项目宣言 (The Why)

Aura OS ADK 是一套为构建**企业级、高可靠、可审计**智能体（Agent）而设计的开发工具包和协议规范。

在 AI 领域， **“思考”** 已经解决，而 **“可信赖的执行”** 是最大的工程挑战。ADK 的使命是提供一个 **安全、分层、可扩展** 的架构，确保无论是金融交易指令还是物理机器人调度，都能被 **高效治理** 和 **精准执行**。

ADK 旨在成为 **Agent OS 生态** 的核心标准，让开发者可以专注于业务逻辑 (Skill)，而不是底层架构。

---

## 🚀 核心特性与优势 (The What & How)

| 特性 | 你的实现 (Code) | 战略价值 (Business Value) |
| :--- | :--- | :--- |
| **任务图协议 (TGP)** | `specs/tgp_v1.0.md` | **定义标准**：将所有复杂任务转化为统一、可编译的 **拓扑结构**。 |
| **治理层壁垒** | `Audit Log`, `Runtime Context` | **核心壁垒**：解决 Agent 行为的**可解释性 (XAI)** 和**责任链 (Accountability)** 难题。 |
| **高度模块化** | 简洁的 **`@aura_plugin`** 装饰器 | **极简开发**：最小化样板代码，让开发者专注于业务逻辑 (Skill)。 |
| **通用调度** | **A3 Tag 体系** | **跨界赋能**：一套代码驱动金融、具身、能源等**任何领域**的 Agent。 |

---

## 🏗️ 架构和协议核心 (The Specs)

### 1. **TGP (Task Graph Protocol) 规范**

TGP 定义了 Agent OS 如何理解一个工作流，是所有 Agent 协作的**底层语法**。

*   **核心文件：** `specs/tgp_v1.0.md` (定义了 Node, Wiring, Manifest 的结构)。

### 2. **ADK 核心接口**

ADK 提供了 Python 抽象接口和装饰器，用于无缝接入 Aura OS 的四大总线：

| 接口 | 作用 | 核心文件 |
| :--- | :--- | :--- |
| **`BasePlugin`** | 所有 Agent 模块的基石，提供生命周期和日志。 | `pdk_interfaces/base_plugin.py` |
| **`@listens_to` / `@publishes`** | 定义 Agent 间的通信规则。 | `pdk_interfaces/decorators.py` |
| **`IEvent` / `IIntent`** | 定义 Agent 协作的语言。 | `pdk_interfaces/interfaces.py` |

---

## 🛠️ 快速上手与生态贡献

### 1. **安装 ADK 接口**

本项目仅包含接口和规范，方便您开始开发。

```bash
pip install aura-os-adk-interfaces