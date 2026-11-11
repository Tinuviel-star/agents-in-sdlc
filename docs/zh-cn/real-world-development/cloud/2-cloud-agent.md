---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。任务完成后，它会创建草稿拉取请求，供开发者审查。这样可以将繁琐任务交给智能体处理，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中所有导出的函数添加 TypeScript 文档注释（TSDoc）。

此外，设计团队希望改进游戏发现体验。他们希望游戏详情页显示相关游戏，也就是相同类别的其他游戏，方便访客继续浏览。他们暂时不需要完善的实现，只需要可用于用户体验验收测试的版本。此事目前受到其他更高优先级议题的阻挡。

这些任务容易被延后，适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者继续做其他工作，之后再回来审查结果。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台处理任务。你可以通过多种方式与它协作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行时或结束后查看它的工作。方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法，不能替代思考。即使看起来简单的操作，也可能包含许多细节。因此，创建任务时应[明确 Copilot cloud agent 的工作范围][cloud-agent-best-practices]。把它当作人工智能（AI）结对程序员：分阶段工作、学习、试验并逐步调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已经完成相关练习，就见过 `.github/copilot-instructions.md` 的效果；否则可先快速查看。分配任务前，只读查看存储库中已有的指令文件，便于稍后识别它们的影响。

在 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将指令应用于 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，留意它创建的拉取请求是否包含 TSDoc 注释、TypeScript 约定和文件头注释。这些指导来自上述指令文件，后续练习会再次检查。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本，确保环境可用。将任务分配给 Copilot 时也一样。

cloud agent 使用 [GitHub Actions][github-actions] 环境处理任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]，在 Copilot 开始工作前安装所需工具和依赖项。实验已预先配置此工作流，以确保 Copilot 能使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库。项目是单个 Astro 应用：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为执行*这些步骤*所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点需要注意：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流，而不必等待 Copilot 启动它。

## 添加文档

许多项目的文档过时或缺失，形成难以处理的技术债务，降低开发效率，也让维护代码库或引入新开发者变得更困难。Copilot 擅长生成文档，因此很适合把这类任务分配给 cloud agent。它会在后台生成文档，后续练习再审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表，因为它正在创建环境（运行前面介绍的工作流）、分析项目并确定处理方式。

13. 查看列表和待办任务。
14. 向下滚动 PR 时间线，确认 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何探索代码、分析当前状态、暂停思考并决定计划，以及创建代码。

这可能需要几分钟。利用 cloud agent 异步工作的特点，接下来同时分配另一项任务，并转而为应用添加功能。

## 在游戏详情页添加相关游戏

Copilot cloud agent 的一个优势是可以并行分配工作：它处理一项任务时，你可以处理其他任务。即使相关游戏区域不需要很长时间，委派给 cloud agent 仍能腾出时间处理其他工作。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 选择 **Assign**。

稍后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作；无需持续监控，完成后再审查即可。接下来创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 cloud agent，让它异步处理技术债务、开发功能或协助迁移代码框架。

在本课中，你：

- 自定义了代码生成环境。
- 了解了如何安全地执行操作。
- 了解了明确界定议题范围的重要性。
- 将议题分配给了 Copilot。

cloud agent 在后台继续工作时，接下来创建并使用自定义智能体。[Copilot cloud agent 也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
