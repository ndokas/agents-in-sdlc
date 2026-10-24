---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在开始对项目进行第一次更改。游戏数据中已有星级评分，但主页上的游戏卡片尚未显示它。让智能体显示评分，审查更改，并将其作为第一个拉取请求合并。

本课将：

- 在功能分支上启动专注的 Copilot 对话；
- 请求智能体对项目进行小幅更改；
- 使用 `/diff` 审查更改；
- 运行应用并在转发的浏览器中确认更改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已显示评分，但主页游戏卡片只显示标题、类别、发行商和说明。现在让智能体在每张卡片上显示现有评分。这项独立的小改动适合作为第一次会话。

## 对话的组成

**对话**是使用 Copilot CLI 处理任务的会话。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用工作树。可以返回已保存的对话；文件和分支仍是磁盘上的普通 Git 状态。

对话中会显示提示词和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

现在启动新对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，在 Mac 上按 <kbd>Control</kbd>+<kbd>`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>`</kbd>。
3. 如 Copilot CLI 尚未运行，执行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 发送以下提示词：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、找到显示游戏详情的文件并生成所需代码。

## 审查差异

合并前需要审查所有 AI 生成的更改，包括小改动。

1. 输入 `/diff` 并检查每个更改文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果；如有失败，请要求它修复。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具具有非确定性，因此实际代码可能不同。应检查行为，而不是期待某种固定实现。

## 检查更改

仅阅读代码并不能确认功能有效。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查 UI。

1. 发送以下提示词，让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示五星制评分。
4. 返回 Copilot 并发送以下提示词，让它停止刚启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能完成后，创建拉取请求 (PR)，将代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 在 Mac 上按住 <kbd>Command</kbd>，或在 Windows/Linux 上按住 <kbd>Ctrl</kbd>，然后选择 Copilot CLI 显示的 URL。
4. 审查已更改文件和检查。
5. 准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

你已完成以下工作：

- 在功能分支上启动专注的 Copilot 对话；
- 要求智能体对项目进行小幅更改；
- 使用 `/diff` 审查更改；
- 运行应用并在转发的浏览器中确认更改；
- 打开并合并第一个拉取请求。

下一步，从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
