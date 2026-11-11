---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本工作坊中，你让 GitHub Copilot 处理了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换。现在审查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

引入生成式人工智能（AI）不会改变软件设计和开发与运维（DevOps）的基本原则。仍需审查生成的代码并遵循正常的 DevOps 流程。接下来审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，然后再为团队启用审查。

## GitHub Copilot cloud agent 安全性

Copilot cloud agent 会异步且自主地执行任务，因此设有安全限制：

- Copilot 只能读取存储库，并且只能写入用于工作的分支。
- cloud agent 在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 审查生成的文档

先通过 github.com 的标准拉取请求（PR）界面审查 GitHub Copilot cloud agent 创建的第一个 PR，其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为默认情况下 Copilot 对外部资源的访问受限，包括访问外部 MCP 服务器。你可以选择[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TypeScript 文档注释（TSDoc）和其他文档。具体更改会有所不同。检查 TSDoc 注释、TypeScript 约定和文件头注释；这些内容来自练习 2 中查看的自定义指令文件。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 查看 PR 检查区域，确认工作流已排队。正常情况下，单个 Astro 应用的项目检查会通过；检查可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以在 PR 评论或代码行内评论中像提及团队成员一样标记 Copilot。Copilot 会读取评论，并启动新会话处理反馈。由于结果具有非确定性，无法提供唯一适用的提示。可以要求 Copilot：

- 为每个代码文件添加简短说明用途的文件头注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 注释。
- 创建说明 Astro 应用结构的 README。

1. 添加评论并标记 **@copilot**，请求它修改生成的文档。使用上述建议之一，或提出其他文档改进要求。
2. 选择 **View Session**，查看 Copilot 如何启动新会话并应用更改。
3. 选择 **Back to pull request** 返回 PR。
4. Copilot 完成修改后，确认 PR 中出现了新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。之后可将 PR 从草稿转换为准备审查状态，再合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

8. 查看 PR 检查区域，确认工作流已排队。正常情况下，单个 Astro 应用的项目检查会通过；检查可能需要几分钟。
9. **可选：**在 Codespace 中切换到 Copilot 创建的分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。你可以像之前一样继续要求 cloud agent 修改，例如调整显示的相关游戏数量，或补充文件头注释和 TSDoc 注释。由于分配任务时尚未更新自定义指令，此 PR 不会自动应用之后添加的指令。可以在 **Conversation** 选项卡中发表评论，要求 Copilot 进行更改。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 重点检查：
   - 用于切换模式的用户界面（UI）组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表（CSS）或样式更改。
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 查看 PR 检查区域，确认工作流已排队。正常情况下，单个 Astro 应用的项目检查会通过；检查可能需要几分钟。
10. **可选：**在 Codespace 中切换到 Copilot 创建的分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍然有效。

无障碍自定义智能体帮助 Copilot 按无障碍最佳实践实现功能。如果发现问题或改进点，可像处理其他 PR 一样在评论中标记 **@copilot**，要求它修改。

## 可选练习：继续委派任务

cloud agent 适合处理待办事项，让你继续做其他工作。可以在存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 cloud agent 工作流。在这些练习中，你：

- 查看了存储库中的自定义指令文件，以了解它们如何影响 cloud agent 的输出。
- 将议题分配给 Copilot cloud agent，观察它设置环境、规划并异步执行任务。
- 创建并使用无障碍自定义智能体，添加高对比度和浅色模式切换。
- 使用 Copilot agents 页面作为控制中心，监控并引导正在运行的无障碍会话。
- 审查并迭代 cloud agent 的 PR，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

cloud agent 工作流已完成。其他工作流可以补充本次练习：

- **[VS Code 工作流](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- **[CLI 工作流](../../cli/)**：从终端交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云委派。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
