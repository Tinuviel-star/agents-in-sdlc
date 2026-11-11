---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者往往需要找到所有相关文件、进行修改、运行测试、调试，再找出遗漏的文件并继续修改。Copilot 智能体模式可以帮助处理这类跨文件工作。

Copilot 智能体模式可以更自主地在集成开发环境（IDE）中工作：探索项目结构、进行必要更改、运行测试等任务，并自动修复发现的问题。接下来使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，适合快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解 Copilot 智能体模式如何：

- 探索项目并找到相关文件，协调多处更改。
- 跨 UI 和数据层实现新功能。
- 在合并前审查 Copilot 生成的更改和测试。

## 场景

游戏列表不断扩大，用户需要按类别和发行方筛选。上一练习已添加发行方辅助函数；现在使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

开始更改前，先查看网站当前功能。

网站是一个以开发者为主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 Drizzle 对象关系映射（ORM）在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行网站。

1. 返回 Codespace。上一练习结束后应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 更新并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中从终端选择 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，此时 Web 服务器正在其中运行。

### 浏览网站

网站运行后，可以查看其主要功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后可查看其详细信息，包括标题、描述、发行方和类别。

## 与 Copilot 一起查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同，相关步骤会注明具体选择。

网站目前可以正常运行，但还需要新功能。通过模板创建项目时会自动生成待办议题，让 GitHub Copilot 帮助查看这些事项。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此将其列入步骤中，以便获得相对一致的体验。由于大型语言模型（LLM）的结果具有概率性，你看到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 输入以下提示，询问 Copilot 待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为事项排序。

## 查看指令文件

生成代码前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在目录，方便复用、拆分复杂指导，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要三项应用更改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并前还必须运行并通过测试。Copilot 智能体模式可以完成这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、找到数据层辅助函数、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟。过程中代码编辑器可能会出现 **Keep** 和 **Undo**。完成后，每项更改都会提供 **Keep** 或 **Undo**，无需在处理中途选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，或有测试暂时失败。这是正常的：Copilot 会在生成代码和运行测试之间迭代，直到任务完成且未检测到错误。

   ![Copilot 智能体模式完成的 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回运行着网站的浏览器，试用新功能。
11. 确认功能正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。在本练习中，你了解了如何：

- 让 Copilot 智能体模式探索项目、定位相关文件并协调更改。
- 使用 Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在合并前审查智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 扩展练习 - 实现分页

随着游戏列表增大，网站需要分页。使用本练习中学到的技能，让 Copilot 添加分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定分页方式，例如是否允许用户选择页面大小，还是使用固定页面大小。
- 在提示中提供 Copilot 所需指导，使其按预期实现功能。
- 可能需要通过提供上下文和要求更改来迭代 Copilot；这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
