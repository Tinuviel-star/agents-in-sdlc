---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就应让它能够获取这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和按路径应用的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）工作流不同，本练习不会比较添加指令前后的提示效果。Copilot cloud agent 会根据 GitHub 议题异步工作，因此难以实时并排展示差异。后续审查 cloud agent 创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式并具有现代风格。
- 代码应包含 TypeScript 文档注释（TSDoc）。
- 每个文件开头应有注释块，说明文件用途。

使用指令文件为 Copilot 提供这些信息，使它按团队实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件，适合存放项目级信息，如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
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

先阅读存储库随附的指令文件：一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些指导适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看内容，其中包括 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它根据相对于存储库根目录的 glob 确定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。它引用了其他指令文件（如 `unit-tests.instructions.md`）和项目文件，可将较大的指令拆分为可复用文件，并链接到代码示例。这里的路径相对于指令文件所在目录，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 注释和文件头注释标准。

## 添加存储库级标准

本练习只需进行一项编辑：添加项目级规则，要求代码使用 TSDoc 注释，并在文件开头添加注释块。之后，cloud agent 会在本工作流后续处理议题时读取这些指令。

编辑前先创建分支。cloud agent 会读取议题目标分支中的指令，而单独提交这项更改也更整洁。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分（大约在第 27 行）。当前部分描述了编码标准，但尚未要求代码文档。
4. 在现有标准后添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可放在存储库级的 `.github/copilot-instructions.md` 中，为所有请求提供指导；也可使用 `*.instructions.md` 针对语言、文件类型或任务提供指导。存储库级文件适合放置刚添加的文档注释等项目标准。

## 提交、推送并合并指令更新

cloud agent 会从议题目标分支读取指令。下一练习中，Copilot 会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。之后 cloud agent 处理下一练习中的议题时，就能读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支完成本工作坊，可以跳过创建分支，直接提交到 `main`。此处创建分支是为了模拟真实项目的处理方式。

## 总结与后续步骤

你了解了 Copilot 如何读取项目指令，并向 `.github/copilot-instructions.md` 添加了存储库级标准。在本练习中，你：

- 探索了项目上下文、编码指南和文档标准如何通过存储库指令和按路径应用的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加了存储库级标准。

后续练习中，cloud agent 创建的拉取请求会体现该标准。

接下来，将第一个议题[分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的五个提示][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
