---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用智能体模式时，Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在本地实时进行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以查看每项差异、接受或撤销单项更改，并通过后续提示引导对话，而无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行时引导智能体添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行时，设计团队又提出添加浅色模式切换的请求，方便觉得默认主题太暗的用户。无需从头开始新对话，只需引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都以内嵌差异显示，并带有 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容，查看每次工具调用，包括读取文件、编辑文件和请求运行的终端命令。
3. Copilot 提出文件编辑时，差异会以内嵌方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式。完成后应会显示更改摘要。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中要求智能体添加浅色模式切换。继续使用同一会话可保留前面工作的上下文，包括修改的组件、样式方案和本地存储方式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续探索已有更改，并提出浅色模式的新增内容。
3. 与高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期（例如尝试重构切换 UI），可使用聊天输入框旁的 **Stop** 按钮停止它，再发送澄清提示。

> [!TIP]
> 与重新开始相比，引导正在进行的会话更快。可以用后续提示要求调整，例如“简化层叠样式表（CSS），改用 CSS 变量而非重复选择器”；纠正方向，例如“不要添加第三个切换项，把浅色模式集成到现有设置面板”；或扩展范围，例如“为两种模式添加键盘快捷键”。

## 审查工作集

提交前快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表，其中应包含 Astro 组件、样式和相关测试。
3. 打开几个已更改文件并查看差异。确认其中体现了自定义智能体要求的无障碍做法，例如无障碍富互联网应用（ARIA）属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并引导了正在运行的会话：逐项审查文件编辑、接受或撤销更改，并通过后续提示扩展任务范围，同时保留原有上下文。

在本课中，你：

- 实时监控了智能体的工作。
- 接受或撤销了单项文件编辑。
- 使用后续提示引导了正在进行的会话。

接下来，在[后续练习][next-lesson]中将无障碍工作作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
