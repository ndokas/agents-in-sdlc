---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式构建了筛选功能。在创建拉取请求前，应先在浏览器中确认功能正常。本练习将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。
本练习将：

- 了解模型上下文协议（Model Context Protocol，MCP）以及 MCP 服务器如何通过新工具扩展 Copilot；
- 向工作区添加 Playwright MCP 服务器；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以借助外部工具扩展能力。模型上下文协议（Model Context Protocol，MCP）是 Copilot 与外部工具和服务通信的标准方式。

![示意图展示智能体模式如何与上下文、大语言模型和工具交互，包括由 MCP 服务器及 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为人工智能（AI）智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时与外部工具和服务通信，利用资源获取最新信息，并通过工具代表用户执行操作。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方通信。外部工具包括现有应用程序编程接口（API）或 NPM 包等本地工具；不同的 MCP 服务器提供不同的一组工具和资源。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)** 提供一组用于管理 GitHub 存储库的 API，允许智能体创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)** 使用 Playwright 提供浏览器自动化能力，允许智能体浏览网页、填写表单和选择按钮。

还有许多 MCP 服务器可以访问其他工具和资源。GitHub 维护 [MCP registry](https://github.com/mcp)，以帮助发现资源并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 用于配置 VS Code 工作区可用的 MCP 服务器。模板已经配置了 GitHub 服务器，因此 Copilot 可以在上一练习中读取议题。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 确认文件中已有以下 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

该配置使用[远程 GitHub MCP 服务器][remote-github-mcp-server]。无需在本地安装，VS Code 会通过 HTTP 连接，并在 Copilot 首次调用相关工具时让你使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

Playwright MCP 服务器为 Copilot 提供可控制的浏览器，用于测试刚完成的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，使文件如下所示：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志让 Playwright 在无可见窗口的模式下运行，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动它并询问你是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 要求确认信任该服务器，请先检查配置，再选择信任以启动服务器。

手动启动是可选的。如果跳过，VS Code 会在下一步首次调用其工具时启动服务器并询问是否信任。

## 测试筛选功能

Playwright MCP 服务器让 Copilot 使用真实浏览器打开页面、导航、应用筛选条件并读取结果，然后总结观察结果。这是在不离开对话的情况下，快速确认功能是否符合预期的方式。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而非截图进行操作。这意味着智能体会依据带标签的结构化元素（例如按钮、链接和列表项）进行推理，就像辅助技术一样。因此，快速功能检查也能兼作基础的无障碍检查。

服务器已连接且应用正在运行后，要求 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会启动浏览器并逐步测试。将结果与议题验收标准对照；如果发现问题，可以继续询问或让 Copilot 修复，然后再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支，以便团队审查更改。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。
   ![显示已更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)
4. 选择 **Publish** 将分支推送到存储库。

接下来让 Copilot 使用 GitHub 工具查找相关议题并创建 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**。
2. 在智能体下拉菜单中选择 **Agent**。
3. 发送以下提示词：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息和执行操作。首次调用 GitHub 工具时，可能需要先进行 GitHub 身份验证。
5. 查看 Copilot 如何搜索议题并创建 PR。
6. 选择 Copilot 返回的链接，查看拉取请求，但暂时**不要合并**。

## 总结与后续步骤

你已完成以下工作：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何通过新工具扩展 Copilot；
- 向工作区添加 Playwright MCP 服务器；
- 使用它在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

接下来，创建[自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
