---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识。团队可以将专业经验整理为可复用的智能体，例如遵循 [Web 内容无障碍指南（WCAG）][wcag]、安全编码实践或一致测试模式的智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 的 YAML Ain't Markup Language（YAML）frontmatter，以及描述智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体与[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并向 AI 说明如何执行操作。最简单的区分方式是：**自定义智能体负责工作，技能提供工具**。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本练习中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它也可以调用拉取请求工作流技能或测试管理技能。

> [!NOTE]
> 自定义智能体没有唯一的正确编写方式。与其他 AI 功能一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

在本练习中，你将：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍功能十分重要，团队希望尽快实现。接下来使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

项目已为无障碍工作准备了自定义智能体。先查看它的内容，了解 Copilot 将如何据此工作。

1. 返回 Codespace，然后查看无障碍智能体文件：

   ```text
   .github/agents/accessibility.md
   ```

2. 查看 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续阅读后续部分，了解其中的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 示例。
   - 常见问题和错误清单。

## 创建并分配议题

GitHub 的智能体控制中心是管理环境中各智能体的中心位置。可以在此分配任务、监控任务并为正在运行的会话提供指导。现在将创建高对比度模式任务并分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![Copilot cloud agent 分配界面中已突出显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结与后续步骤

本课介绍了 GitHub Copilot [自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义智能体，团队可以将专业知识和标准整理成可复用的指导。

在本课中，你：

- 了解了自定义智能体的定义方式。
- 将任务分配给了自定义智能体。

Copilot 正在实现高对比度模式。接下来，从智能体控制中心[监控并引导会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体的准备工作][org-custom-agents]
- [在企业中使用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
