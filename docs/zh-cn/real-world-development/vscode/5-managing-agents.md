---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

启用 GitHub Copilot 代理模式后，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示工具调用和文件编辑。你可以直接在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，除了高对比度模式外再添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作尚未完成时，设计团队又提出一项需求：为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话；你将引导现有会话继续扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每一步，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内嵌差异的形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，浏览聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内嵌方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式。完成后会显示总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中继续添加浅色模式切换。继续当前会话可保留前序工作的上下文，包括已修改的组件、采用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会继续工作，检查已有更改并提出添加浅色模式的修改。
3. 与高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体采用了不符合预期的方案（例如尝试重构切换界面），可以选择聊天输入框旁边的 **Stop** 按钮停止它，然后发送澄清说明。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以通过后续提示要求调整（“简化层叠样式表 (CSS)，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换项，将浅色模式加入现有设置面板”）或扩展范围（“为两个切换项添加键盘快捷键”）。

## 审查工作集

提交前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查全部已更改文件。应能看到 Astro 组件、样式和相关测试更新。
3. 打开部分已更改文件并查看差异。确认其中体现了自定义智能体的无障碍模式，包括可访问富互联网应用 (ARIA) 属性、键盘导航、语义化 HTML，以及通过本地存储持久化设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导进行中的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本练习中，你：

- 实时监控智能体的工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导进行中的会话。

接下来，在[迭代改进 Copilot 的工作][next-lesson]中，将无障碍功能发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
