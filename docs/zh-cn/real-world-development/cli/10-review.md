---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 GitHub Copilot 命令行界面（CLI）工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot 命令行界面（CLI）。在整个过程中，你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，随后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖范围、技能结果和浏览器验证证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选与质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为简化工作坊，这些更改合并到了同一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 综合检查结果、需求和测试覆盖率审查，再供 PR 使用。

新增测试应解决实际覆盖缺口；如果 QA 检查后认为无需新增测试，也可能是正确结果。授权合并前审查代码和证据；更改后应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复任务，自定义智能体定义专业角色，MCP 服务器连接外部工具。应检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方案，使用 **Interactive** 参与专注的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选速度较快的模型，复杂工作则可选能力更强的模型。

上下文和基础设施同样重要。明确描述要构建什么、为什么构建以及如何实现，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以了解以下 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

熟练使用工具的最佳方式是持续实践。可以将它用于生产代码、个人项目，或一直想做却尚未着手的小应用。与团队分享经验，也向团队成员学习。还可以继续阅读文档。

如果想从终端扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
