---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

前两项练习中，你让 Copilot cloud agent 处理了三项改善用户体验和添加功能的任务。cloud agent 会自主异步工作，但监控任务仍然很重要。

可以使用多种工具管理已分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有具有未关闭拉取请求（PR）的智能体任务、查看已执行的操作，并为正在运行的会话提供指导。

在本练习中，你将：

- 使用智能体页面监控 cloud agent 任务。
- 引导正在运行的会话添加功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，可以引导它同时实现浅色模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表，其中应包括：
   - 更新代码库文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置模型上下文协议（MCP）服务器。
   - 完成任务的其他步骤。

> [!NOTE]
> 页面中显示的具体步骤会因 Copilot 任务的状态和执行方式而异。

4. 同时查看右侧显示的 PR 窗格，其中可以查看 PR 和更改文件。

## 引导 cloud agent

了解正在进行的任务后，要求 Copilot 在实现高对比度模式时同时添加浅色模式。

1. 选择正在添加高对比度模式的会话。会话标题因 Copilot 使用的名称和任务状态而异。

   ![智能体控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。之后会看到类似的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的 cloud agent 任务，以及突出显示的“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已收到提示，并将其纳入任务。

## 等待 Copilot 完成

Copilot 会继续处理更新后的任务。收到新消息后，它会先完成当前步骤，再将新请求加入工作流程。

这可能需要几分钟，可以利用这段时间回顾前面练习的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。可以在此跟踪进行中和已完成的任务、审查工作，并调整正在运行任务的范围。

在本课中，你：

- 使用智能体控制中心和智能体页面监控了 cloud agent 任务。
- 引导正在运行的会话添加了功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
