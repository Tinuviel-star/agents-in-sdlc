---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，再通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot 命令行界面（CLI）可以配合组织现有的开发流程，帮助团队以合适的方式构建正确的功能。本课将开始一组智能体驱动开发练习：从 GitHub 议题出发生成新功能，验证代码和功能行为，最后将更改成功合并到项目中。

> [!NOTE]
> 后续功能练习会沿用同一对话和分支。实际工作中，通常会按不同文件类型拆分分支或 PR；本工作坊为了聚焦核心概念，会简化流程。

在本课中，你将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行方筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。使用 Plan 模式敲定这些决定，再授权 Autopilot 按明确范围实现功能。

## 背景

在开发流程中引入 AI 编码智能体不会改变软件开发的基本原则，反而使这些原则更重要。多数开发者的流程大致如下：

1. 从说明需求的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成（CI）成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准流程，可确保 AI 生成的代码满足需求，并像人工编写的代码一样经过审查和验证。

## 对话模式

**对话模式**决定智能体可以自主完成多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，但不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直到认为任务完成。

先切换到 Plan 模式并审查计划，然后再使用 Autopilot 实现。

## 从议题开始

开始筛选工作前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

此时提示中应出现 `#7`（或类似编号）。`#` 可将 GitHub 议题或拉取请求加入 Copilot 的上下文。

## 规划筛选功能

规划能让你在交由 Copilot 执行前，先确定功能实现或任务处理方式。复杂任务尤其适合先花时间规划。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方式，制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答；这里没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划与已决定的方案不符，请让 Copilot 修改相应部分。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现。Autopilot 会让 Copilot 按计划迭代，直到认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中名称相似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。审查代码并运行网站，确认结果符合预期。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时确定的方案检查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果；如果有失败，请让 Copilot 修复。

## 试用新功能

代码看起来没问题后，还需确认它确实可以运行。让 Copilot 启动应用，再通过 Codespaces 转发的端口访问网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 试用类别筛选、发行方筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题和计划中的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你使用不同的对话模式构建并审查了功能。在本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中明确了需求。
- 使用 Autopilot 模式实现了功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，使用[自定义指令][next-lesson]进一步引导代码生成，使其遵循文档中说明的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
