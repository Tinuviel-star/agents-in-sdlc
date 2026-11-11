---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你可以在 GitHub 上分配任务，cloud agent 会在后台处理：探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

在这些练习中，你将添加 cloud agent 会遵循的自定义指令，然后分配 GitHub 议题并让它实现任务。你还将审查并使用自定义智能体来引导工作方式，从智能体控制面板监控和引导会话，最后审查 cloud agent 创建的拉取请求并迭代结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud agent][ex2] | 异步智能体 | 将议题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查拉取请求（PR）、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？经过验证的学生可通过 [GitHub Education][student-plan] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、模型上下文协议（MCP）、代码审查和 Copilot CLI 功能。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise** 计划，不适用于 Copilot Free。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用此功能。

> [!NOTE]
> 本工作流不涉及 MCP。要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 工作流][cli-harness]或 [VS Code 工作流][vscode-harness]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
