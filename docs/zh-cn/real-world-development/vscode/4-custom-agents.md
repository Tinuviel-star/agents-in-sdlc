---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识。团队可以将专业经验整理为可复用的智能体，例如遵循 [Web 内容无障碍指南（WCAG）][wcag]、安全编码实践或一致测试模式的智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 的 YAML Ain't Markup Language（YAML）frontmatter，以及描述智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体与[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并向 AI 说明如何执行操作。最简单的区分方式是：**自定义智能体负责工作，技能提供工具**。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它也可以调用拉取请求工作流技能或测试管理技能。

> [!NOTE]
> 自定义智能体没有唯一的正确编写方式。与其他 AI 功能一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。

在本练习中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用此智能体实现高对比度模式。

由于无障碍功能十分重要，团队希望尽快实现。接下来使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

项目已为无障碍工作准备了自定义智能体。先查看它的内容，了解 Copilot 将如何据此工作。

1. 返回 Codespace，在终端中切换到基于 `main` 的新分支，以便将无障碍工作与练习 3 的筛选拉取请求（PR）分开：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. 打开 `.github/agents/accessibility.md`。
3. 查看 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

4. 阅读后续部分，了解其中的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 示例。
   - 常见问题和错误清单。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择智能体后，该会话将遵循相应指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同，相关步骤会注明具体选择。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表。默认情况下，当前智能体为 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 这可能需要几分钟。Copilot 会修改存储库中的真实文件，例如 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot [自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义智能体，团队可以将专业知识和标准整理成可复用的指导。

在本课中，你：

- 查看了自定义智能体的定义方式。
- 在 Copilot Chat 智能体模式中使用了自定义智能体。

接下来[监控并引导智能体工作][next-lesson]，在审查更改的同时，要求它在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
