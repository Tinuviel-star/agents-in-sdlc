---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就应让它能够获取相关上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和按路径应用的指令文件传递给 Copilot。
- 使用当前指令生成第一个筛选数据切片（发行方辅助函数）。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选代码切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能与部分标准不一致。Copilot 的结果具有概率性。本练习要观察的是更新指令后输出行为的整体变化趋势，而不是要求每个字符都与示例一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代风格。
- 代码应包含 TypeScript 文档注释（TSDoc）。
- 每个文件开头应有注释块，说明文件用途。

通过指令文件为 Copilot 提供这些信息，使它按团队实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件，适合存放项目级信息。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot 命令行界面（CLI）和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。详情请查看[自定义指令支持参考][custom-instructions-support]。

### 管理指令文件的最佳实践

示例项目展示了一种有代表性的指令组织方式：

- 将 `copilot-instructions.md` 限定为项目级指导，如项目结构和全局编码标准。
- 使用 `*.instructions.md` 指导特定文件类型（例如单元测试、Astro 组件和数据层）或任务。
- 使用自然语言，保持说明清晰，并提供代码示例。

指令文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的指令文件。项目中包含 UI 更新和 Astro 等多种任务的指令，可查看 [UI 指令][ui-instructions]和 [Astro 指令][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关练习会在需要时说明具体入口。
>
> 想找模板或起点？浏览包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先查看存储库随附的指令文件：一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些指导适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看内容，其中包括 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它根据 glob 确定指令适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 打开 `.github/instructions/drizzle.instructions.md` 并查看其中指向其他指令文件和项目文件的链接。这样可以拆分较大的指令，并链接到可参考的示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 注释和文件头注释标准。

## 为更改创建分支

先为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同，相关步骤会注明具体选择。

先在当前指令下发送提示，观察生成结果；之后更新指令并再次发送相同提示，以便比较差异。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应放置的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 输入以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改辅助函数和测试文件。
6. 查看建议的辅助函数：它应是一个带类型的函数，将 `db` 客户端作为第一个参数，并返回带类型的发行方数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意建议代码中**没有** TSDoc 注释和文件头注释。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有指令，也可能生成文档注释。如果发生这种情况也没关系，本练习重点仍是观察添加指令后行为的一致性变化。

## 添加存储库级标准

在 `.github/copilot-instructions.md` 中添加项目级标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约位于第 27 行）。注意其中尚未要求代码文档，这也是初始生成的辅助函数没有文档注释的原因。
3. 在现有标准后添加以下内容，要求 Copilot 添加文件头注释和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可放在存储库级的 `.github/copilot-instructions.md` 中，为所有请求提供指导；也可使用 `*.instructions.md` 针对语言、文件类型或任务提供指导。存储库级文件适合放置刚添加的文档注释等项目标准。

## 再次运行提示并观察变化

再次运行相同提示，比较 Copilot 是否遵循新标准。

1. 返回 Copilot Chat 并选择 **New Chat**，清空上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 输入与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看建议文件开头是否出现类似以下注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行方数据访问辅助函数。
    * 提供用于从数据库检索发行方信息的函数。
    */
   ```

5. 查看建议函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回包含 ID 和名称的发行方列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行方对象数组的 Promise。
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到后续练习会继续使用的代码中。

## 提交指令并推送分支

指令文件与其他项目文件一样，通过源代码管理进行维护。提交并推送此分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新和 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

   如果 `git status` 显示其他配套更改（例如测试），也要将这些文件加入暂存区。
4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行方辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. 等待 Copilot 查找议题并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR，并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许它代表你执行 GitHub 操作和 shell 命令，请审查命令，并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结与后续步骤

你了解了 Copilot 如何读取指令文件，并使用 VS Code 中的 Copilot Chat。在本练习中，你：

- 探索了项目上下文、编码指南和文档标准如何传递给 Copilot。
- 在现有指令下生成了第一个发行方辅助函数，并观察了结果。
- 向 `.github/copilot-instructions.md` 添加了存储库级标准。
- 再次运行相同提示，观察建议的代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的五个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
