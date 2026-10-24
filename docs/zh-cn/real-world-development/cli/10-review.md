---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。具体来说，你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目并找到预置的筛选议题；
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查；
- 使用自定义说明引导智能体，自定义现有 `quality-checks` 技能，并运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中检查筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，检查需求、测试覆盖、技能结果和浏览器证据；
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR；
- 探索用于上下文、模型、共享和可选云委派的斜杠命令。

## 交付的更改

本研讨会包含两个 PR 阶段，分别从更新后的 `main` 创建分支：

1. **星级评分：**在游戏卡片上显示已有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选，更新说明并应用于功能，自定义 `quality-checks` 报告，创建 QA 配置，并加入相应测试。

从规划筛选功能到创建 PR，你沿用同一对话和分支。本研讨会将这些工作放在一个 PR 中以简化流程。

## 不同类型的验证

你使用自动化测试、浏览器手动检查和 Copilot 通过 MCP 进行浏览器检查验证代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖检查结合起来，形成 PR 前的报告。

新增测试应填补实际覆盖缺口；如果 QA 检查后发现无需添加测试，也可能是合理的。更改代码后，重新检查相关证据；合并授权前，审查代码和证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。本研讨会中，你更新了说明、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在不同对话中复用这些自定义配置，并根据团队需求调整。说明定义标准，技能描述可重复任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。审查实际更改和工具结果，而不只是智能体摘要。

根据任务选择**模式和模型**：使用 **Plan** 先确定方案，使用 **Interactive** 逐步处理聚焦的更改，使用 **Autopilot** 执行范围明确的任务。日常编辑可选择响应更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**构建以及**如何**构建，会显著影响输出。

## 更多功能

核心工作流已完成，还可以了解其他 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取不同角度的建议。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离独立任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最好方式是持续使用它。可以将 Copilot 用于生产代码、个人项目，或一直想构建却尚未开始的小应用。与团队分享经验并向团队学习，也要持续查阅文档。

要从终端继续扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列课程][foundry]。要比较其他工作环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
