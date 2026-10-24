---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并在浏览器中检查筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码还需要处理数据、外部服务，并为 Copilot 提供自动化工具。模型上下文协议（Model Context Protocol，MCP）服务器可以扩展 Copilot CLI 内置的能力，让它使用更多工具和服务。
本课将：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 让智能体驱动浏览器并检查筛选功能。

## 场景

单元测试和端到端测试很重要，但验证用户界面（UI）更新还需要实际与其交互。现在让 Copilot 像用户一样使用网站，进一步自动验证更改是否符合预期。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议（MCP）][mcp-blog-post]为人工智能（AI）智能体提供与外部工具和服务通信的方式。借助 MCP，智能体可以实时访问最新信息并代表用户执行操作。MCP 服务器充当智能体与外部工具和服务之间的桥梁，并提供各自的一组工具和资源。

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

这两个是常见的 MCP 服务器。还有许多其他服务器；GitHub 维护 [MCP registry][mcp-registry]，以提高资源的可发现性并促进生态系统贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请审查源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请将它添加到配置中。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入服务器名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 在 Mac 上按 <kbd>Control</kbd>+<kbd>S</kbd>，或在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>S</kbd>，保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 检查功能

之前已手动确认功能正常。现在让 Copilot 使用 Playwright MCP 服务器完成相同检查。

1. 发送以下提示词：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

   > [!NOTE]
   > Copilot 通常可以自行判断何时使用 MCP 服务器，但在提示词中指出工具有助于提高结果一致性，并节省少量令牌。

2. 观察 Copilot 如何通过浏览器逐项验证功能。
3. 阅读报告并确认行为符合预期。Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并汇报结果。

## 总结与后续步骤

你已完成以下工作：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它；
- 在需要时添加 Playwright MCP 服务器；
- 让智能体驱动浏览器并检查筛选功能。

接下来，创建[质量保证 (QA) 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
