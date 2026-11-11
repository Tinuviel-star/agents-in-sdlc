---
title: "第 6 课 - 使用 Playwright 模型上下文协议（MCP）验证功能"
description: "在 Copilot 命令行界面（CLI）中配置 Playwright MCP，并通过浏览器检查筛选行为。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。还需要处理数据和外部服务，并让 Copilot 能够使用更多自动化工具。模型上下文协议（MCP）服务器可以扩展 Copilot CLI 的内置功能，为它提供其他工具和服务。

在本课中，你将：

- 了解模型上下文协议（MCP）及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让智能体操作浏览器，检查筛选功能。

## 场景

单元测试和端到端测试很重要，但验证用户界面（UI）更新还需要实际与界面交互。通过 Playwright MCP，Copilot 可以像用户一样操作网站，进一步自动验证更改是否符合预期。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让人工智能（AI）智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁；每个服务器提供一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

此外还有许多 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助开发者发现资源并为生态系统做出贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样审查 MCP 服务器。使用前检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

你已手动确认功能行为。现在让 Copilot 使用 Playwright MCP 服务器完成同样的检查。

1. 输入以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断应使用 MCP 服务器，不一定需要在提示中指定。不过，明确指出要使用的工具有助于获得更一致的结果，也能减少额外提示。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认功能行为符合预期。

Copilot 会启动服务器，通过 Playwright 操作网站，完成后停止服务器并提供报告。

## 总结与后续步骤

你通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中检查了功能。在本课中，你：

- 了解了模型上下文协议（MCP）及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 让智能体操作浏览器并检查了筛选功能。

接下来，创建一个[质量保证（QA）自定义智能体][next-lesson]，将技能和浏览器工具结合到专门的工作流程中。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [在 Copilot CLI 中添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
