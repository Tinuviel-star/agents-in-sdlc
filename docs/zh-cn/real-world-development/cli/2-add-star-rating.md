---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 GitHub Copilot 命令行界面（CLI）修改游戏卡片，在转发的浏览器中检查结果，并将更改作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 GitHub Copilot 命令行界面（CLI）并尝试过一次对话后，现在可以开始第一次项目更改了。先从小任务入手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。让智能体显示已有评分，审查更改，然后通过第一个拉取请求合并。

在本课中，你将：

- 在功能分支中启动专注于当前任务的 Copilot 对话。
- 请智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认结果。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示评分。不过，主页上的游戏卡片目前只显示标题、类别、发行方和描述。作为热身练习，让智能体在每张卡片上显示已有评分。这项更改范围小、相互独立，很适合作为第一次会话练习。

## 对话的组成

**对话**是使用 Copilot CLI 处理任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话可供日后继续；文件和分支仍是磁盘上的普通 Git 状态。

对话中包含三类信息：你发送的提示和智能体的回复、智能体探索并编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

新建对话来实现这项功能。开始前返回 Codespace，并在 Copilot CLI 中创建新会话。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot CLI 尚未运行，执行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确认已启动新会话。
5. 输入以下提示，请求 Copilot 修改项目：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详情的文件，并创建所需代码。这样就通过 Copilot CLI 添加了第一项功能。

## 审查差异

所有人工智能（AI）生成的更改都应在合并前审查，即使更改很小也一样。使用 Copilot CLI 检查更改内容和验证结果。

1. 输入 `/diff` 并检查每个更改文件。
2. 确认游戏卡片会在评分存在时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果；如果有失败，请让 Copilot 修复。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果具有概率性，并非确定性。代码可能与示例不同；应检查实际行为，而不是要求实现与示例完全一致。

## 检查更改

不要只看代码就假定功能正常。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并让它停止所启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成，接下来创建拉取请求（PR），将代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

你已使用 GitHub Copilot CLI 交付第一项更改。在本课中，你：

- 在功能分支上启动了专注于当前任务的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查了更改。
- 运行应用，并在转发的浏览器中确认了星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
