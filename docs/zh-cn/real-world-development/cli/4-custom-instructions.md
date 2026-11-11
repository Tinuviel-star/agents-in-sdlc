---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令，添加代码文档标准，并将新标准应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要按特定方式完成，就应让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应如何组织。完成筛选功能后，查看 Copilot 使用的指令，添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解存储库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发指南和要求，包括：

- 注释应解释意图和不明显的决定，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript 文档注释（TSDoc）或 JavaScript 文档注释（JSDoc）说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码变更时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过指令文件提供这些信息，确保 Copilot 按照团队实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件，适合存放适用于大多数聊天或命令行界面（CLI）请求的项目级信息。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

在修改前，先查看初始项目中已有的指令文件，了解当前指导及后续变化。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于该存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹，查看其中的指令文件，包括 Astro 文件、Drizzle 数据层和测试相关指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它定义了一个 glob，用于确定指令适用的文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
6. 查看该文件中针对本项目单元测试的说明。
7. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。它引用了其他指令文件（如 `unit-tests.instructions.md`）和项目文件，可将较大的指令拆分为可复用文件，并链接到代码示例。这里的路径相对于指令文件所在目录，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting guidance** 部分描述了项目编码标准，但目前没有要求为代码添加文档。接下来会添加 TSDoc 注释标准。

## 更新指令文件以符合团队标准

现有文件已经提供了良好的起点，但仍缺少一项标准。修改核心文件 `copilot-instructions.md`，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存指令后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 按更新后的指导调整代码：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看更改的 TypeScript 文件。确认新增的 TSDoc 注释能准确说明代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何读取指令文件，并将新标准应用到筛选功能。在本课中，你：

- 查看了存储库指令和按路径应用的指令文件。
- 更新了指令文件以要求遵循代码文档标准。
- 观察了指令文件对代码的影响。

接下来，自定义并运行可复用的 [quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
