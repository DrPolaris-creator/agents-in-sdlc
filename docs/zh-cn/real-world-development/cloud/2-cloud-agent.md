---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因一直没有时间实现而积压的功能请求。GitHub Copilot 云智能体能够自主执行代码更新和功能开发等任务。任务完成后，它会创建草稿拉取请求 (PR)，供开发者审查。这样可以将繁琐任务委派出去，加快开发进度，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责创建网站的承包商没有为代码添加文档。作为第一步，团队希望为应用程序中所有导出的函数添加 TypeScript 文档注释 (TSDoc)。

此外，设计团队已准备好改进游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，即同一类别中的其他游戏，方便访客继续浏览。目前不需要完善的实现，只需要用于验收测试用户体验 (UX) 的版本。该工作目前受阻，但还有优先级更高的议题。

这些任务很容易被搁置，适合分配给 Copilot 云智能体异步处理。开发者可以继续处理其他任务，之后再回来审查 Copilot 的工作并确认结果符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看它的工作。提出的解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确界定说明范围的重要性

GitHub Copilot 没有魔法，也不能替你思考。看似简单的操作，深入了解后也可能很复杂，因此需要[谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。应将它视为 AI 结对编程伙伴：分阶段工作、学习、试验并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已完成自定义说明练习，就已经看过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务给 Copilot 云智能体前，先只读查看存储库中已有的说明文件，以便之后观察它们的影响。

在存储库的 GitHub Web 用户界面 (UI) 中，或在已启动的 Codespace 中查看以下文件：

1. 打开 `.github/copilot-instructions.md`，查看 **Code standards** 部分，尤其注意 TypeScript 约定和 TypeScript 文档注释 (TSDoc) 要求。
2. 打开 `.github/instructions/unit-tests.instructions.md`，查看 `applyTo` front matter。它将说明范围限定为 `**/*.test.ts` 文件。

下一节将把 `Code lacks documentation` 议题分配给云智能体。留意生成的 PR 是否遵循了这些说明中的 TSDoc 文档注释、TypeScript 约定和文件注释头。后续练习还会再次检查这些内容。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，以确保开发环境状态正常。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师 (SWE) 的方式完成工作。

云智能体在执行任务时使用 [GitHub Actions][github-actions]。你可以创建专用设置工作流来自定义环境，并配置 `.github/workflows/copilot-setup-steps.yml`，使其在 Copilot 开始工作前运行。这能让智能体使用所需开发工具和依赖项。本实验已预先配置此工作流，方便练习并确保流程顺畅。它会为这个单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为这些步骤所需的最低权限。Copilot 会为自己的操作获得单独的令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤中不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（应用现在由 Astro + Drizzle/libSQL 构成）
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

      # 迁移并填充本地 SQLite 数据库，确保构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几个关键点：

- 工作流包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会执行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以先测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

尽管大家都知道文档的重要性，但许多项目的文档要么已经过时，要么完全缺失。这类技术债务往往得不到处理，会降低开发效率，并增加维护代码库或帮助新开发者加入团队的难度。Copilot 擅长生成文档，因此很适合将此议题分配给 Copilot 云智能体。它会在后台生成所需文档；后续练习将审查其工作。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建空白议题。
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
11. 打开新创建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定解决问题的方案。

13. 检查列表及其计划执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、理解当前状态、暂停思考并确定适当计划，以及编写代码。

整个过程可能需要几分钟。Copilot 云智能体的主要优势之一是能异步执行任务，让开发者专注于其他工作。现在利用这一点：再分配一项任务给 Copilot 云智能体，然后转而编写应用程序的新功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以分工：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分不会花很长时间，这段时间也可以用来处理其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
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

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题中的第一条评论会出现 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示它正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。Copilot 云智能体的工作方式类似软件工程师，因此不必一直监控；等它完成后再审查结果即可。现在继续创建并使用自定义智能体。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也支持模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并能使用前面练习介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
