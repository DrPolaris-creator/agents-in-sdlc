---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

前几节练习中，你已让 Copilot 云智能体处理了三项任务，目标是改善用户体验并添加功能。云智能体虽然可以异步自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。这个任务控制中心会显示所有具有未关闭拉取请求 (PR) 的智能体任务。你可以查看已执行的操作，也可以在会话进行中引导智能体。

在本练习中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在运行的会话，请求添加额外功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也应该添加浅色模式。由于网站样式和模式切换功能已经在修改中，添加这项功能很合适。现在引导智能体，同时实现浅色和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到已分配给 Copilot 的任务，包括：
   - 为代码库添加文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和所采用的方案而异。

4. 同时查看右侧显示的拉取请求 (PR) 窗格，其中可以查看 PR 和更改文件，以便进一步监控任务。

## 引导云智能体

查看当前任务后，让 Copilot 在实现高对比度模式的同时添加浅色模式切换。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始实际工作。出现类似以下消息时，表示已进入此阶段。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，其中高亮显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成发送消息时正在执行的步骤，再将新请求整合到工作流程中。

这个过程可能需要几分钟。可以趁此机会回顾目前学到和探索的内容。

## 总结和后续步骤

本练习介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以在此跟踪所有进行中和已完成的任务、查看工作内容，甚至重定向正在运行的任务以调整范围或提供补充指导。

你了解了以下内容：

- 浏览任务控制中心和智能体页面，监控云智能体任务。
- 重定向正在运行的会话，请求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
