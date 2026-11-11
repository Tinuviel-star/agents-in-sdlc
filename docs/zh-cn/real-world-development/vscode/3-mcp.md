---
title: "练习 3 - 使用 Playwright 模型上下文协议（MCP）服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求（PR）前，应先在浏览器中确认功能正常。接下来连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器，在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

智能体模式连接编辑器之外的工具后会更加强大。MCP 是 Copilot 与这些工具通信的标准方式，可让智能体连接外部工具和服务。

![智能体模式内部工作原理示意图，展示其与上下文、模型和工具的交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让人工智能（AI）智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以实时获取最新信息（通过资源）并代表你执行操作（通过工具）。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器负责管理智能体与外部工具和服务（例如现有应用程序编程接口（API）或本地 npm 包）之间的通信；每个服务器提供一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，智能体可以创建和更新存储库、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，智能体可以访问网页、填写表单和选择按钮。

此外还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助开发者发现资源并为生态系统做出贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样审查 MCP 服务器。使用前仔细检查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 确认其中已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板已包含 `github` 配置，因此上一练习中 Copilot 能够读取 GitHub 待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时使用 GitHub 账户进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数让 Playwright 在后台运行，不显示浏览器窗口；Codespace 没有桌面环境，因此需要此参数。

> [!NOTE]
> Tailspin Toys 已将 Playwright 用于端到端测试，通常已经安装了所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请先审查配置，再选择信任以启动服务器。

手动启动是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要工具时启动服务器并提示信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果，无需离开当前对话。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体依据带标签的结构化元素（按钮、链接和列表项）进行判断，方式与辅助技术类似。这样，简单的功能检查也能兼顾基本的无障碍检查。

连接服务器并启动应用后，让 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器，逐项检查功能并报告结果。对照议题中的验收标准审阅报告。如果行为不符合预期，可继续提问，或让 Copilot 修复代码后再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以发布 `filtering-vscode` 分支并创建 PR，供团队审查。

1. 打开 Codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。也可以让 Copilot 查找相关议题并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 查找信息并执行操作。首次使用 GitHub 工具时，可能会提示使用 GitHub 进行身份验证；按照提示授权。
5. 确认 Copilot 已搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**不要合并**。

## 总结与后续步骤

在本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前让 Copilot 使用它在浏览器中手动测试了筛选功能。
- 发布了分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来创建[自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
