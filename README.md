# 🤖 Aura OS - Agent Development Kit (ADK)

**The Universal Execution and Governance Standard for AI-Native Agents.**  
*面向企业级自主智能体（Agent）的统一执行协议、分布式运行治理与开发工具包。*

---

[![Powered by OrcaRouter](https://img.shields.io/badge/Router-OrcaRouter-0070f3?style=flat-square&logo=fastapi)](https://www.orcarouter.ai/)
[![Model-Agnostic](https://img.shields.io/badge/LLM-Any%20OpenAI--Compatible-green?style=flat-square)](https://github.com/zqcchris/Aura-OS-ADK)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=flat-square)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10%2B-brightgreen?style=flat-square)](https://www.python.org/)

---

## 💡 项目宣言 (The Why)

Aura OS ADK 是一套专为构建**企业级、高可靠、可审计**智能决策系统而设计的底层开发框架与协议规范。

在生成式 AI 时代，大模型的“单点思考”已经解决，但如何让智能体在真实的复杂商业场景中实现**“确定性、可信赖、高并发的工程执行”**，是当前最大的技术瓶颈。

ADK 旨在成为 **Agent OS 时代的标准基础设施**：
* 无论是驱动金融高频交易、企业级 C 端 24 小时数字员工（AI 码），还是实体具身机器人的物理协同；
* 开发者只需专注于核心业务逻辑（Skills 与 Plugins），底层的通信调度、任务编排、多轮上下文与审计追踪全由 Aura 内核接管。

---

## 🚀 核心架构与特性 (Core Features)

| 特性维度 | 实现机制 (Mechanism) | 战略价值 (Business Value) |
| :--- | :--- | :--- |
| **任务图协议 (TGP)** | `specs/tgp_v1.0.md` | **声明式编排**：将复杂业务转化为拓扑图（Node 与 Wiring），零硬编码实现 Agent 热插拔。 |
| **ATP 话题总线** | `comms/bus/protocol.py` | **工业级调度**：基于严格 6 段式原语（Stream/Event/Inquiry/Act/View），实现背压与优先级调度。 |
| **分布式会话记忆** | `plugins/session_memory.py` | **海量高并发**：内存零泄露，基于 Redis Pipeline 紧凑滑动窗口，支撑数万用户并发对话。 |
| **审计治理壁垒** | `Audit Log` & `WorldModel` | **企业级合规**：内置因果推演与全生命周期审计，解决 Agent 责任链与可解释性难题。 |
| **极简插件化开发** | `@aura_plugin` 装饰器 | **极致开发体验**：一行装饰器完成事件监听、发布声明与配置校验。 |

---

## 🌐 多模型路由与生态接入 (Model Ecosystem)

Aura OS 坚持 **模型无关（Model-Agnostic）** 的设计哲学，底层网关全面兼容 **OpenAI 标准协议**，支持企业自带算力（BYOK - Bring Your Own Key）。

```
       ┌────────────────────────────────────────────────────────┐
       │                 Aura OS Execution Engine               │
       └───────────────────────────┬────────────────────────────┘
                                   │ (OpenAI-Compatible Protocol)
       ┌───────────────────────────┴────────────────────────────┐
       ▼                                                        ▼
【聚合智能路由 (Recommended)】                          【直连私有模型 / 厂商】
* OrcaRouter (零成本免费模型/自动容灾)                     * DeepSeek / 阿里通义百炼 (Qwen)
* 自建 OneAPI / NewAPI                                   * Anthropic Claude 3.5 / OpenAI
```

### 🌟 推荐模型路由：OrcaRouter

本项目原生集成了 **[OrcaRouter](https://www.orcarouter.ai/)** 提供的统一大模型智能路由服务：
* ⚡ **零成本开箱体验**：内置对 `orcarouter/free` 免费模型池的原生支持（可调用 DeepSeek V4 Flash、GLM 5.3 Flash、腾讯混元等，调用成本 $0/token，极适合开发测试）。
* 🔀 **高可用故障转移**：在并发高峰或网络波动时，由路由层秒级自动重试与转移，确保企业级 Agent 永不宕机。

#### 示例配置：`config/provider.yaml`
```yaml
# Aura OS 统一模型服务商配置文件
model_provider: "orcarouter"
model: "orcarouter/auto" # 生产环境自适应路由，或调试用 "orcarouter/free"

model_providers:
  orcarouter:
    name: "OrcaRouter"
    base_url: "https://api.orcarouter.ai/v1"
    wire_api: "responses"
    env_key: "ORCA_KEY"
```

---

## 🏗️ 架构拓扑规范 (The Specs)

### 1. 蓝图定义 (Blueprint DSL)
每一个智能体由声明式 YAML 或 DSL 蓝图驱动，例如标准 24h AI 接待员：

```yaml
agent_id: "ai_receptionist_officer"
agent_name: "Aura企业数字员工"
domain: "commerce"

nodes:
  - id: "constant_node"
    type: "context_suite_node"      # 业务知识库
  - id: "memory_node"
    type: "session_memory"           # 分布式短期记忆中枢
  - id: "reception_engine"
    type: "ai_reception"             # 无状态大模型推理引擎

wiring:
  # 用户原始消息 ➔ 记忆节点前置拦截注入
  - subscribe: "aura://stream/public/+/+/chat/v1"
    to: "memory_node"
    bus: "system"
  # 记忆节点 ➔ 接待引擎推理
  - from: "memory_node"
    subscribe: "aura://stream/public/+/+/chat_enriched/v1"
    to: "reception_engine"
    bus: "local"
  # 接待引擎 ➔ 记忆节点持久化存盘
  - from: "reception_engine"
    subscribe: "aura://event/commerce/+/+/chat_archived/v1"
    to: "memory_node"
    bus: "local"
```

### 2. ATP (Aura Topic Protocol) 寻址原语
系统内部所有神经元交互遵循 6 段式协议：
`aura://{Archetype}/{Domain}/{Org}/{Identity}/{Aspect}/{Version}`
* **`stream`**：低延迟高频数据流与对话流；
* **`event`**：状态变更因果反馈；
* **`act`**：最高执行优先级的指令执行件。

---

## 🛠️ 快速上手 (Quick Start)

### 1. 环境准备
```bash
git clone https://github.com/zqcchris/Aura-OS-ADK.git
cd Aura-OS-ADK
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. 配置环境变量 (`.env`)
```bash
# 拷贝模板
cp .env.example .env

# 配置你的大模型 API 密钥 (以 OrcaRouter 为例)
ORCA_KEY="sk-your-orcarouter-key"
ORCA_BASE_URL="https://api.orcarouter.ai/v1"
DEFAULT_MODEL="orcarouter/free"
```

### 3. 开发你的第一个 Agent 插件
```python
from app.kernel.sdk.plugin_base import BasePlugin
from app.kernel.sdk.pdk import aura_plugin, listens_to, publishes
from app.kernel.sdk.contracts.messages.base import BusType

@aura_plugin(
    name="my_first_plugin",
    version="1.0.0",
    description="示例业务插件"
)
class MyPlugin(BasePlugin):
    @listens_to("aura://stream/public/+/+/chat/v1", bus=BusType.SYSTEM)
    @publishes("aura://event/commerce/+/+/alert/v1", bus=BusType.LOCAL)
    async def on_event(self, payload: dict):
        self.log(f"接收到用户输入: {payload.get('message')}")
        yield "aura://event/commerce/org/guest/alert/v1", {"status": "SUCCESS"}
```

---

## 🤝 生态伙伴 (Partners & Ecosystem)

本项目由以下开源友好型伙伴支持：

*   [![OrcaRouter](https://img.shields.io/badge/Built%20with-OrcaRouter-0070f3?style=flat-square)](https://www.orcarouter.ai/ref/ref_f553dc2c0dc08641605e) 

    **[OrcaRouter](https://www.orcarouter.ai/ref/ref_f553dc2c0dc08641605e)** —— 为 Aura OS ADK 提供多模型统一路由与开发者免费算力支持。

---

## 📄 开源许可证 (License)

本项目遵循 [Apache 2.0 开源协议](LICENSE)。