---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏文件并再次修改，如此反复。

GitHub Copilot 代理模式就是为此而设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它会像开发者一样先探索项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体名称相似，但它们针对不同场景。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和发行商筛选。上一练习已添加 publishers helper，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改前先了解 Tailspin Toys 网站当前的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，用户可以浏览游戏列表和详情。它是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端 API 或客户端用户界面 (UI) 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能通过使用标准文档对象模型 (DOM) 应用程序编程接口 (API) 的小型、作用域受限的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一个练习结束后，你应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果对话框提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，选择 Codespace 终端中的 localhost URL 会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到运行网站的 Codespace 的专用隧道。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后会进入详情页，查看标题、描述、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 下方步骤会创建一个全新对话。各练习使用的模式和模型可能不同；具体步骤会在相应位置说明。

网站的初始实现已可正常运行，但还需要添加新功能。先查看待办事项。使用项目模板时，系统已自动创建一组议题；请 GitHub Copilot 显示这些议题。

1. 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始全新对话，然后在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列为操作步骤，以便尽可能保持代码建议一致，提升体验。不过，由于大型语言模型具有概率性，收到的建议可能与示例不同。这是正常现象。

> [!NOTE]
> 由于大型语言模型 (LLM) 具有概率性，Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度，帮助确定议题优先级。

## 查看说明文件

让智能体生成代码前，先查看用于向 Copilot 提供上下文的说明文件。本练习将使用 [user interface (UI)][ui-instructions] 文件，其中包含为网站添加功能时应遵循的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、将复杂说明拆分为较小部分，并为 Copilot 提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三类更改：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选 UI。

此外，将更改合并到代码库前，测试必须运行并通过。可以让 Copilot 代理模式完成这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件，包括数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo**，因此无需在工作进行时选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 窗格提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能会有测试失败。这是正常的。Copilot 会在生成代码和运行测试之间来回迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终审查 Copilot 或任何 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 在将更改合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为该功能创建拉取请求。

### 额外探索练习——实现分页

随着游戏列表增长，网站需要支持分页。使用本练习学到的技能，请 Copilot 更新网站以实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页方式：是否允许用户选择每页大小，还是将其固定。
- 编写提示时提供必要指导，让 Copilot 按你的需求实现功能。
- 可能需要通过后续提示和补充上下文迭代修改。这是与 Copilot 协作的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
