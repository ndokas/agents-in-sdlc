---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在智能体模式下，GitHub Copilot 会自主工作：探索代码库、提出更改、编辑文件并运行命令。由于这些工作在你的计算机上实时执行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示词引导对话，以完善或扩展工作，而无需离开编辑器。

本练习将：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在同一会话中引导智能体添加浅色模式切换，并保留高对比度模式。

## 场景

练习 4 中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出增加浅色模式的要求。无需从头开始新对话，可以指导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 会显示智能体执行的步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内嵌差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 使用的 Copilot Chat 视图。
2. 在智能体工作时查看聊天内容，观察工具调用、文件读取、文件编辑和终端命令。
3. 对每项文件编辑，选择 **Keep** 接受更改，或选择 **Undo** 撤销更改并让智能体重新处理。
4. 选择聊天顶部的 **Files changed** 指示器，查看本次会话的完整更改文件列表。

   > [!NOTE]
   > 如果工具调用要求许可（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式，并查看它对所做更改的总结。

## 指导智能体添加浅色模式

继续同一对话，让智能体在高对比度模式旁添加浅色模式。由于保留了之前的上下文，智能体可以沿用已修改的组件、样式方法和本地存储实现。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 查看智能体如何检查现有更改并提出浅色模式更新。
3. 像处理高对比度更改一样，逐项审查编辑并选择 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期（例如不必要地重构切换 UI），可以选择聊天输入框旁的 **Stop**，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示词要求调整或扩展工作，例如：“简化 CSS，使用 CSS 变量而不是重复选择器”（"simplify the CSS — use CSS variables instead of duplicating selectors"）、“纠正方向：不要添加第三个切换开关，将浅色模式整合到现有设置面板中”（"don't add a third toggle, integrate light mode into the existing settings panel"），或“扩展范围：为两个切换开关添加键盘快捷键”（"also add keyboard shortcuts for both toggles"）。

## 审查更改文件

提交之前，快速检查智能体修改的所有文件。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看更改文件列表，其中应包含 Astro 组件、样式和相关测试。
3. 打开部分文件并审查差异，确认更改体现了自定义智能体中的无障碍模式，包括可访问的富互联网应用程序（ARIA）属性、键盘导航、语义化 HTML，以及使用本地存储持久保存设置。
4. 确认更改后，在 Source Control 面板中暂存并提交。下一课将发布这些更改。

## 总结与后续步骤

你已使用 Copilot Chat 智能体模式监控并指导正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示词扩展工作。

本练习中，你了解了以下内容：

- 实时监控智能体工作；
- 接受或拒绝单项文件编辑；
- 使用后续提示词在进行中的会话中引导智能体。

下一课将把无障碍更改发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
