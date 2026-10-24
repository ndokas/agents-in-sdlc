---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定实现方案，使用 Autopilot 根据议题构建筛选功能，再使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚才已为项目添加一项小功能。较大的更改需要更完善的流程。本课将从 GitHub 议题开始，按照智能体驱动的开发流程规划、构建、审查并手动验证筛选功能。之后还会更新存储库说明、自定义 `quality-checks` 技能、添加模型上下文协议（MCP）验证、创建质量保证（QA）智能体并打开功能拉取请求（PR）。

> [!NOTE]
> 继续完成此功能时，会沿用同一对话和分支。实际工作中通常会为不同文件类型使用多个分支或 PR；本研讨会为聚焦核心概念而简化流程。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和发行商筛选游戏。待办议题描述了该功能，但组合类别等细节需要先确定。使用 Plan 模式讨论决定，再授权 Autopilot 执行范围明确的实现。

## 背景

将人工智能（AI）编码智能体纳入开发流程不会改变基本原则。事实上，这些原则变得更加重要。大多数开发者遵循类似以下的流程：

1. 从已提交的议题开始，了解需要完成的工作。
2. 为要构建的内容制定计划。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数都是上述流程的变体。

遵循标准流程有助于确保 AI 生成的代码满足要求，并经过与手写代码相同的审查。

## 对话模式

**对话模式**决定智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在模式之间切换：

- **Interactive**：与智能体协作；智能体提出更改并等待输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直到认为任务完成。

先在 Plan 模式中审查计划，再使用 Autopilot 实现。

## 从议题开始

先确认 Codespace 和存储库已准备就绪。

1. 返回 Codespace。如果已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，在 Mac 上按 <kbd>Control</kbd>+<kbd>`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>`</kbd>。
4. 更新 `main` 并创建筛选分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示词并返回 **Session** 选项卡。提示词中会出现类似 `#7` 的引用；`#` 可将 GitHub 上的议题或拉取请求加入上下文。

## 规划筛选功能

规划可在交由 Copilot 执行前确定实现方案。对于复杂任务，先花时间规划通常很有帮助。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式，并确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并制定计划。

3. Copilot 可能会询问筛选功能的行为细节。根据自己的偏好回答。
4. 计划完成后，在 Mac 上按 <kbd>Control</kbd>+<kbd>E</kbd>，或在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>E</kbd>，展开计划。
5. 上下滚动并审查计划。
6. 要求 Copilot 修改任何不符合决定的部分。

## 批准 Autopilot

审查计划后，让 Copilot 执行实现。

1. 选择 **Accept plan and build on autopilot (recommended)**，或安装版本中含义相同的选项。
2. 确认模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此先审查计划。如果 Copilot 报告依赖项缺失或端口冲突，请先解决环境问题，再判断检查结果。

## 审查并验证实现

和其他代码一样，生成的代码也必须经过审查后才能合并。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式，并确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与议题和规划时作出的决定对照。
4. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并让 Copilot 修复失败项。

## 体验新功能

启动应用并通过 Codespaces 转发端口在浏览器中打开网站。

1. 发送提示词，让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商以及计划中约定的组合进行筛选。
4. 确认重置和无结果状态符合议题及计划中的决定。
5. 返回 Codespace，要求 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已完成以下工作：

- 从 GitHub 议题启动 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现功能；
- 在 Interactive 模式中审查代码；
- 在转发的浏览器中手动验证功能。

下一步，使用[自定义说明][next-lesson]了解 Copilot 如何生成代码并遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
