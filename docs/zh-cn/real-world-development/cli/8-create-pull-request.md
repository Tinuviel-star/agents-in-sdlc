---
title: "第 8 课 - 创建并合并功能拉取请求（PR）"
description: "一起审查筛选功能、指令、技能更新、质量保证（QA）配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在一起审查这些更改并创建拉取请求（PR）。之前你亲自合并了星级评分 PR；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际工作中，通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个 PR 中。为简化本工作坊，我们让完整的筛选和质量工作流共用一个对话和分支，并将所有更改放进同一个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在完成审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## Agent Merge 简介

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回应审查意见，以及在需要时变基；之后，如果存储库设置允许，Copilot 会启用 GitHub 自动合并。

此前由你自行选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍应审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码完成后，一起审查更改、创建 PR，并让 Agent Merge 处理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似 `PR #14 was squash-merged successfully.` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因；在存储库允许的情况下，完成审查后再手动合并。

## 总结与后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理 PR。在本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，探索用于管理上下文、模型、共享和可选云委派的实用 [Copilot CLI 斜杠命令][next-lesson]。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
