---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索存储库说明，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 应了解某些背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]用于提供这类指导，让 Copilot 不仅了解要做*什么*，也了解代码应*如何*组织。筛选功能已经构建完成；现在查看 Copilot 使用的说明，添加文档标准，并将其应用于代码。

在本课程中，你将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript 文档注释 (TSDoc) 或 JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持最新。
- 保留现有格式和 lint 指导。

使用说明文件可确保 Copilot 获取正确的信息，并按照这些实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定首选编码约定、库，以及需要添加的注释类型。既可以为整个存储库创建说明，也可以针对特定文件类型提供任务级指导。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。该文件应包含项目级信息，即大多数请求都适用的上下文。
- `.github/instructions/*.instructions.md`：可为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目中的自定义说明文件

示例项目已包含一组说明文件。在进行更改前，先浏览现有文件，了解它们的作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。这些说明适用于与 Copilot 的所有存储库交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了用于决定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明拆分成更小、可复用的文件，并为 Copilot 提供可参考示例。

## 更新说明文件以符合团队指导

现有文件已是良好起点，但还有一处需要补充。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置应在第 35 行附近。
2. 将以下内容作为该部分最后一条项目符号添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存说明后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并逐一查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将一项新标准应用于功能代码。具体来说，你：

- 了解了存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
