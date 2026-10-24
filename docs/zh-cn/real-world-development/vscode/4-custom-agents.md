---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专用人工智能（AI）助手。通过在存储库 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以向 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识。团队可以将经验整理为可复用智能体，例如执行 [Web Content Accessibility Guidelines（WCAG）][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或确保测试模式一致的测试智能体。

项目级智能体定义在 `.github/agents` 文件夹中，也可以在 `~/.copilot/agents` 中全局定义。每个文件都有 YAML front matter，至少包含 `name` 和 `description`，后面是定义智能体行为、专长和说明的 Markdown 提示词。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。简单来说，**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体有自己的上下文窗口，可在工作中协调使用技能，甚至其他智能体。本实验中的无障碍自定义智能体会依据无障碍准则审查并更新网站，也可以调用拉取请求工作流或运行、管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。需要通过测试和迭代找到适合环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文本与背景颜色对比度不足，有些用户觉得当前深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户启用和关闭的高对比度模式。

无障碍至关重要，因此我们希望尽快实现此功能。你将使用自定义智能体生成该功能。

本练习将：

- 审查现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

先从 `main` 创建新的无障碍工作分支，避免与筛选 PR 混在一起；然后检查智能体配置，了解其职责和指导。

1. 打开终端并运行以下命令，创建并切换到工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. 打开 `.github/agents/accessibility.md`。
3. 查看包含 `name` 和 `description` 字段的 YAML front matter。

   > [!CAUTION]
   > 自定义智能体必须包含带有 `name` 和 `description` 的 front matter。

4. 浏览后续部分，了解生成无障碍网站代码时的核心职责、无障碍最佳实践、HTML 和层叠样式表（CSS）/JavaScript 示例，以及常见陷阱。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的自定义智能体。选择自定义智能体后，该会话会使用它的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始干净的对话。模式和模型选择可能因练习而异，相关步骤会说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)
2. 选择 Chat 视图底部的智能体下拉菜单（默认显示 **default**）。
3. 从列表中选择 **Accessibility agent**。
4. 发送以下提示词：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 将探索代码库、提出更改并修改项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑不断应用，查看 **Files changed** 指示器的更新，了解智能体修改的文件。

   > [!NOTE]
   > 这可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一练习会审查并指导这项进行中的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，这类智能体是面向特定任务和领域的专用 AI 助手。使用自定义智能体可以将团队经验和标准整理为可复用内容，从而更有效地指导 Copilot 完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并指导智能体工作][next-lesson]，查看更改过程，并要求它在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在集成开发环境（IDE）中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright 模型上下文协议（MCP）服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
