---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 应了解某些背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]用于提供这类指导，让 Copilot 不仅了解要做*什么*，也了解*如何*完成。

在本练习中，你将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面 (CLI) 工作坊不同，这里不会先后运行提示来比较结果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时对比。稍后审查云智能体创建的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应使用深色模式，并具有现代感。
- 应使用 TypeScript 文档注释 (TSDoc) 为代码添加文档。
- 每个文件顶部都应添加一段注释，说明文件用途。

使用说明文件可确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它了解编码风格和需求。可以指定首选编码约定、库，以及需要添加的注释类型。既可以为整个存储库创建说明，也可以针对特定文件类型提供任务级指导。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。该文件应包含项目级信息，即大多数 Copilot 聊天或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建，用于指导特定语言（如 TypeScript 或 Astro），或特定任务（如创建 UI 组件或新增一组单元测试）。

> [!NOTE]
> 在集成开发环境 (IDE) 中工作时，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明以及带 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持采用其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

本工作坊不会全面讨论如何创建说明文件，但示例项目展示了一种具有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过实验找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览项目中的文件时，可以看到其中针对多种任务提供指导，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应工作坊会在适用时介绍这些入口。
>
> 想寻找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读存储库附带的说明文件：一个核心 `copilot-instructions.md`，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 用户界面 (UI) 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于决定这些说明适用的文件。这里会匹配任何 TypeScript 测试文件，例如匹配 `**/*.test.ts` 的文件。
5. 查看其中针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中链接到其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分成更小的可复用文件，并为 Copilot 提供可参考示例。（其中路径是相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来只需编辑一次：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续练习中，云智能体处理议题时会读取这项指导。

编辑前，先创建分支。云智能体会从议题目标分支读取说明，但仍应通过分支提交更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置应在第 27 行附近。注意其中记录了项目编码标准，但尚未加入代码文档规则，因此生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可以放在存储库级文件 `.github/copilot-instructions.md` 中，提供全局指导；也可以创建为 `*.instructions.md` 文件，为特定语言、文件类型或任务提供说明。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中，分配给 Copilot 的议题会从 `main` 创建分支，因此要让云智能体读取更新后的说明，必须先将更改合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，云智能体处理下一练习分配的议题时就能读取这些说明。

> [!TIP]
> 如果本工作坊中想直接在 `main` 上操作，可以跳过创建分支，直接提交到 `main`。创建分支是为了让工作坊流程贴近真实项目的做法。

## 总结和后续步骤

你已完成以下工作：

- 了解 Copilot 如何从本项目的说明文件中获取上下文。
- 向 `.github/copilot-instructions.md` 添加了一项存储库级标准。

后续课程中，可以观察云智能体创建的拉取请求如何体现这项标准。接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
