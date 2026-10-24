---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot 命令行界面（CLI）构建，并与 GitHub 原生集成，因此存储库、分支和持续集成（CI）管道可以直接使用。应用适用于同时指挥多个智能体、自动执行重复任务的工作流；每个智能体都在隔离的工作区中运行。安装 Node.js 并准备好项目副本后，接下来安装应用并连接存储库。

本课将：

- 安装 GitHub Copilot app 并登录；
- 从 GitHub 存储库将项目添加到应用；
- 熟悉工作区，包括模板创建的待办事项；
- 尝试快速聊天，了解应用本身。

## 场景

团队正在采用人工智能（AI）智能体来处理不断增加的待办事项。Copilot app 提供了统一工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将帮助你完成安装和连接，并熟悉如何发起有关项目的对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。

## 安装并配置 GitHub Copilot app

应用支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证，并添加 Tailspin Toys 存储库。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按照产品页面上的说明安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用支持数据驻留的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果初始设置流程提供存储库选择，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过此步骤，在完成初始设置后添加项目。
6. 按提示选择主题，然后完成初始设置。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已列出，无需再次添加。
8. 如果尚未连接存储库，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。使用先决条件课程中记录的所有者和存储库名称。
9. 确认侧边栏中显示存储库。在此处使用的布局中，`tailspin-toys` 等存储库文件夹位于 **Sessions** 下，各存储库的会话显示在相应文件夹下面。

## 熟悉工作区

连接项目后，熟悉侧边栏中的各个区域：

- **New**：启动新的 Copilot 聊天会话。
- **Pull requests**：通过应用原生 GitHub 集成浏览、筛选和审查拉取请求，并检查 CI 状态。
- **Issues**：浏览和筛选议题，也可以从议题启动会话。
- **Automations**：保存可按计划或按需运行的智能体任务，适合管理待办事项、定期维护和其他重复性工作。总结部分会将它作为后续方向，而不是另一项工作坊练习。
- **Customize**：通过模型上下文协议（MCP）服务器、插件、技能和其他组件为应用添加功能。本研讨会将使用此区域配置 Playwright MCP。
- **Chats**：用于提问和头脑风暴的轻量对话，无需为其单独创建分支或工作区。开始第一次聊天后，该区域才会显示。如果未显示，请选择 **Sessions** 旁的 **+** 启动第一次聊天。
- **Sessions**：按存储库组织文件夹和智能体会话。选择 `tailspin-toys` 等存储库名称打开存储库视图，或选择其下的会话返回对话。每个会话都在独立工作区运行，因此可以并行工作而不会造成更改冲突。下一课将创建第一个会话。

在完成工作坊的过程中，你会逐步探索工作区。

> [!TIP]
> 不确定如何操作或某项功能是否可用时，可以直接询问 Copilot。

### 查找预置的待办事项

通过模板创建项目时，系统会自动创建一组待办议题。现在查看这些议题。

1. 在侧边栏的 **Sessions** 下选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题并阅读其详细信息。每个议题也可以作为智能体会话的启动点；完成一项快速的首次更改后，将从筛选议题启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 列表会自动筛选，只显示已添加到 Copilot app 的存储库中的项目。要查看其他存储库中的工作项，请将那些存储库添加到应用。

## 尝试快速聊天

快速聊天无需创建分支或工作树，适合一次性提问或头脑风暴。

1. 在侧边栏中选择 **Chats** 旁的 **+**，打开新聊天。如果 **Chats** 尚未显示，选择 **Sessions** 旁的 **+** 启动第一次聊天；之后 **Chats** 会显示出来。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 git 工作树中运行，因此可以同时运行多个智能体，而不会造成更改冲突。你可以随时继续对话或开始新聊天。

## 总结与后续步骤

你已完成以下工作：

- 安装 GitHub Copilot app 并登录；
- 从 GitHub 存储库将项目添加到应用；
- 熟悉工作区，并在 **Issues** 中找到预置待办事项；
- 使用快速聊天提出一次性问题。

接下来，[启动第一个智能体会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
