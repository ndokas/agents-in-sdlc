---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行相应操作，例如运行测试。开发者通常需要先找出所有相关文件、进行更改、运行测试、调试、查明遗漏的文件并再次修改，工作流程往往还不止这些。

这正是 Copilot 智能体模式可以发挥作用的地方。它可以在 IDE 中更自主地工作，类似开发者那样先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们面向不同的工作方式。智能体模式在 IDE 中执行任务，便于快速反馈和互动。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将了解：

- Copilot 智能体模式如何探索项目、找出相关文件并协调多处更改；
- GitHub Copilot 智能体模式如何跨用户界面（UI）和数据层实现新功能；
- 如何在将代码合并到代码库前审查智能体模式生成的更改和测试。

## 场景

随着游戏列表增长，希望允许用户按类别和发行商筛选。上一练习已添加 publishers helper；现在使用智能体模式完成剩余的数据层、用户界面（UI）和测试工作。

## 运行 Tailspin Toys 网站

在更改代码前，先了解 Tailspin Toys 网站当前的功能。该网站是一个以开发者为主题的棋盘游戏众筹平台，允许用户浏览游戏列表及其详情。它是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle 对象关系映射（ORM）在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能使用调用标准文档对象模型（DOM）API 的小型 Astro `<script>`。

### 启动网站

先检查当前网站，然后为筛选工作创建分支并启动开发服务器。

1. 返回 Codespace。上一个练习结束后应位于 `main`。
2. 打开终端：在 Mac 上按 <kbd>Control</kbd>+<kbd>`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>`</kbd>。
3. 创建并切换到筛选工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. 按住 Mac 上的 <kbd>Command</kbd> 或 Windows/Linux 上的 <kbd>Ctrl</kbd>，并选择终端中的 `http://localhost:4321` 地址。

   > [!NOTE]
   > 如果 Codespace 提示打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭对话框。Codespace 会将该地址重定向到 `https://<your-codespace-name>-4321.app.github.dev/`，这是通向 Codespace 的私有隧道。

### 浏览网站

网站运行后，可以浏览其主要功能：

- **主页：**显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页：**选择游戏后会进入详情页，其中包含游戏标题、说明、发行商和类别。

## 使用 Copilot 查看待办事项

先在干净的 Copilot Chat 会话中查看模板自动创建的 GitHub 议题。

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始干净的对话。模式和模型选择可能因练习而异，步骤会在需要时说明。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。此智能体可以自主规划并实现跨文件更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)
2. 从可用模型中选择 **Claude Sonnet 4.5**。

   > [!CAUTION]
   > 本研讨会作者不偏好某个模型。构建本研讨会时使用了 Claude Sonnet 4.5，希望提高体验一致性。由于大语言模型（LLM）具有概率性，收到不同建议是正常现象。

   > [!NOTE]
   > Copilot 可能使用不同的模型上下文协议（MCP）工具命令，但仍应能完成任务。

3. 发送以下提示词，查看存储库中的议题并帮助确定优先级：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为议题排序。

## 查看说明文件

实现代码前，先查看 `.github/instructions/ui.instructions.md`，了解 Copilot 添加网站功能时应遵循的架构、组件设计、可测试性和无障碍指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中的总体指导及其指向 Astro、Tailwind CSS 和 Drizzle 说明文件的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件位置，因此可以复用指导、拆分复杂说明，并提供示例和模板。

## 实现筛选功能

完成筛选需要至少更新应用的三个部分：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，将更改合并到代码库前，测试必须运行并通过。Copilot 智能体模式可以完成这些任务。现在开始添加功能。

1. 继续当前 Copilot 会话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图仍选择 **Agent**，并且仍选择 **Claude Sonnet 4.5**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 根据待办事项中的相关议题，让 Copilot 实现功能。发送以下提示词：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. 观察 Copilot 如何先探索项目并定位相关文件，包括数据层 helper、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

   > [!NOTE]
   > Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。处理过程中，代码编辑器中可能会出现 **Keep** 和 **Undo**。Copilot 完成后，所有更改都会有 **Keep** 或 **Undo** 选项，因此无需在工作进行时选择。
6. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认是否运行测试的截图](../../../_images/ex3-agent-mode-run-tests.png)

7. 等待 Copilot 在代码生成和测试之间反复迭代。过程中可能会暂停，或暂时看到测试失败，这些情况都没有问题。Copilot 会持续调整代码并运行测试，直到完成任务且未发现错误。

   ![Copilot 智能体模式完整会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)
8. 检查生成的代码，留意潜在问题。

   > [!CAUTION]
   > 始终审查 Copilot 或其他人工智能（AI）工具生成的代码。

9. 返回运行网站的浏览器，体验新功能。
10. 确认功能正常且代码审查完成后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 让 Copilot 智能体模式探索项目、找出相关文件并协调多处更改；
- 在将代码合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为更改创建拉取请求。

### 附加探索：实现分页

随着游戏列表增长，需要实现分页。使用本练习中学到的方法，提示 Copilot 更新网站并实现分页。编写提示词时，可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 决定分页方式，例如是否允许用户选择每页大小，还是使用固定值；
- 提供 Copilot 所需的实现指导，使其按预期完成工作；
- 与 GitHub Copilot 迭代协作，要求修改并补充上下文。这是使用 Copilot 的正常流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
