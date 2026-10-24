---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你已让 GitHub Copilot 云智能体处理了三项改善用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监控任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在此 Mission Control 中，可以查看所有具有未关闭拉取请求的智能体任务、检查已执行的操作，并指导进行中的会话。

本课将：

- 使用智能体页面监控云智能体任务；
- 指导正在进行的会话添加其他功能。

## 场景

分配高对比度模式任务后，团队决定同时添加浅色模式。由于正在更新网站样式并添加切换功能，因此可以让智能体同时实现这两种模式。

## 查看 Copilot 云智能体任务

先查看分配给云智能体的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表应包括更新代码文档、生成游戏修改 API 和添加网站高对比度模式等任务。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如检出代码、创建工作环境、设置 MCP 服务器以及完成任务的其他步骤。

   > [!NOTE]
   > 具体显示的步骤会根据任务状态和 Copilot 采用的方法而变化。

4. 查看右侧的拉取请求窗格，其中显示 PR 和已更改文件，便于继续监控。

## 指导云智能体

现在让 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。标题会根据 Copilot 使用的名称和当前工作状态而不同。

   ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始处理任务。此时会开始显示类似下方示例的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 查看 Copilot 如何确认提示词并将其纳入工作流程。

## 等待 Copilot 完成工作

Copilot 会在当前步骤完成后，将新的要求纳入任务并继续处理。此过程需要几分钟，可以利用这段时间回顾之前学到的内容。

## 总结与后续步骤

本课探索了 Copilot agents 页面，这是监控和指导云智能体任务的中心位置。你可以跟踪进行中和已完成的任务、审查工作过程，并调整正在进行的任务。

你了解了以下内容：

- 使用 Mission Control 和 agents 页面监控云智能体任务；
- 引导正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
