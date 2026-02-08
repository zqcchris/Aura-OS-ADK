
# 🤝 贡献指南 (Contributing Guide)

欢迎加入 Aura OS ADK 社区！您的贡献将直接帮助定义下一代 AI 基础设施的标准。

---

## 1. 核心贡献哲学

我们的核心原则是：**「结构化、可审计、向后兼容」**。

请确保所有贡献都符合 **TGP (Task Graph Protocol)** 的规范。我们更看重代码的**清晰度**和**可靠性**，而不是单纯的功能数量。

---

## 2. 贡献领域 (Where to Contribute)

您的贡献主要集中在以下三个方面：

### 2.1 协议与规范 (The Specs) - **高价值贡献**

*   **目标：** 完善和升级 TGP 规范 (`/specs` 目录)。
*   **内容：** 提出 **Aura 协议升级提案 (AUP)**，例如：增加新的控制流 (`if/else`) 逻辑，或优化 A3 Tag 体系。

### 2.2 核心接口 (The Interfaces) - **高门槛贡献**

*   **目标：** 改进或扩展核心抽象接口 (`/pdk_interfaces` 目录)。
*   **内容：** 优化 `BasePlugin` 抽象类，或改进 `@listens_to` 装饰器的性能。

### 2.3 技能与示例 (The Skills & Examples) - **最受欢迎贡献**

*   **目标：** 贡献可复用的、高质量的 Skill 插件示例。
*   **内容：** 编写新的 **Bridge Plugins** (例如：`MqttBridgePlugin` 连接物理世界) 或 **Data Processing Skills** (例如：`AdvancedKalmanFilter`).

---

## 3. 贡献流程 (The Process)

### 3.1 编码规范

1.  **Python 版本：** 推荐使用 Python 3.10+。
2.  **格式化：** 请使用 `Black` 或 `Ruff` 进行代码格式化。
3.  **测试：** 任何贡献都应附带简单的单元测试，以证明其符合 TGP/PDK 接口契约。

### 3.2 提交规范 (Pull Request)

1.  **提交问题 (Issue)：** 在提交 PR 之前，请先提交一个 **Issue** 来描述您要解决的问题或添加的功能。
2.  **创建分支：** 从 `main` 分支拉出您的功能分支 (`feature/your-feature-name`)。
3.  **遵循规范：** 提交代码后，请确保您的插件符合 **TGP v1.0** 的规范。
4.  **标题：** PR 标题应清晰描述您的改动，例如：`feat: Add MqttBridgePlugin for physical world connection`。

### 3.3 协议贡献 (AUP 提案)

*   如果您想修改核心 TGP 规范，请在 `/specs/aup-proposals` 目录下提交一个新的 Markdown 文件。
*   核心团队和社区将进行审阅、讨论和表决。

---