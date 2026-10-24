---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因没有时间实现而长期搁置的功能请求。GitHub Copilot 云智能体旨在自主完成更新代码和添加功能等任务。完成后，它会创建草稿拉取请求，供开发者审查。这样可以把繁琐任务交出去，加快开发进程，并让开发者专注于更重要的整体工作。

本练习将了解如何：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘创建网站的承包商没有留下合适的文档——实际上，代码完全缺少文档。首先，他们希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。此时不需要精致完善的实现；他们只需要一个可用于用户体验验收测试的版本。这项工作目前受阻，但还有优先级更高的议题。

这两项工作都可能很快被降低优先级，因此适合交给 Copilot 云智能体。云智能体可以异步处理任务，让开发者先专注于其他工作，之后再回来审查 Copilot 的结果并确认符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可在后台执行任务，类似于人类开发者。你可以通过[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)启动任务。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理。你可以在会话进行中或完成后查看过程；准备好审查方案时，Copilot 会在拉取请求中通知你。

## 明确说明任务范围

使用 GitHub Copilot 并不能跳过思考。即使看起来简单的操作，深入分析后也可能很复杂，因此[应仔细界定云智能体的任务范围][cloud-agent-best-practices]。将它视为人工智能（AI）结对程序员，分阶段工作、学习、试验并根据情况调整。生成式 AI 不会改变软件开发的基本原则。

## 查看存储库中的自定义说明

之前的练习介绍了自定义说明如何引导 Copilot。如果尚未亲自操作过，现在可以先只读查看本存储库的说明，之后便能识别它们对生成结果的影响。

1. 在存储库的 GitHub 网页界面中，或已启动的 Codespace 中打开 `.github/copilot-instructions.md`，查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
2. 打开 `.github/instructions/unit-tests.instructions.md`，查看 `applyTo` front matter，它将说明限定到 `**/*.test.ts` 文件。
3. 在下一部分将 `Code lacks documentation` 议题分配给云智能体后，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。稍后的练习还会再次审查这些内容。

## 为云智能体设置开发环境
无论由谁编写代码，通常都需要特定环境并运行设置脚本，确保一切处于良好状态。将任务分配给以类似软件工程师（SWE）方式工作的 Copilot 时，也需要进行环境设置。

云智能体使用 [GitHub Actions][github-actions] 环境完成任务。可以通过在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，自定义环境并在开始工作前运行。这样可为智能体提供所需的开发工具和依赖项。本实验已提前配置好该工作流，以便流程顺利进行并让你体验此功能。它会为云智能体提供 Node.js、项目依赖、端到端测试所需的浏览器，以及已迁移并填充数据的单个 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（整个应用现已基于 Astro 和 Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来和其他 GitHub 工作流文件一样，但有几个关键点：

- 它包含名为 `copilot-setup-steps` 的单个作业，该作业会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器：它允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以先验证工作流能否成功运行，而不必等待 Copilot 启动它。

## 添加文档任务

虽然大家都知道文档的重要性，但大多数项目要么信息过时，要么完全缺少文档。这类技术债务往往无人处理，会降低工作效率，也让维护代码库或引入新开发者更加困难。幸运的是，Copilot 擅长创建文档，这正是适合分配给云智能体的议题。它会在后台生成所需文档，你将在后续练习中审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)
9. 选择 **Assign**。
   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求 (PR)，其标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新的 PR，请稍等片刻并刷新浏览器窗口。
12. 等待几分钟，直到 Copilot 创建待办事项列表。
   > [!NOTE]
   > Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定解决问题的最佳方法。
13. 查看列表及 Copilot 计划完成的任务。
14. 向下滚动 PR 时间线，查找 Copilot 已开始处理议题的更新。
15. 选择 **View session**。
    ![Copilot 会话视图](../../../_images/ex4-view-session.png)
   > [!CAUTION]
   > 你可能需要刷新窗口才能看到更新后的指示器。
16. 浏览实时会话，观察 Copilot 如何探索代码并了解当前状态、暂停思考并选择合适的计划，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来我们会利用这一点：先把另一项任务分配给云智能体，然后转而为应用编写代码、添加功能。

## 添加相关游戏区域

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组任务。为设计团队添加相关游戏区域可能不需要太久，但这段时间仍可用于其他工作。现在将这项任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)
9. 选择 **Assign**。
   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)
短时间后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理。
   ![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)
Copilot 正在认真处理第二项请求！云智能体的工作方式与软件工程师类似，因此你无需主动监控；等它完成后再审查即可。现在将注意力转向创建和使用自定义智能体。

## 总结与后续步骤

本练习探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理任务。你可以使用 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你了解了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以把注意力转向创建和使用自定义智能体。[Copilot 云智能体还可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并支持自定义说明；我们已在之前的模块中探索过自定义说明。

接下来，创建并使用[自定义智能体][next-lesson]。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
