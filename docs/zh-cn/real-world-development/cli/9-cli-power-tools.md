---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI 提供许多斜杠命令，用于访问高级功能、查看内部信息和配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。本课将继续探索其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 浏览可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在再了解管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]等功能。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型上下文窗口上限。Copilot CLI 需要时会自动压缩对话，也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及系统说明、工具、消息和剩余空间的上下文分布。
4. 输入以下命令压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口逐渐填满时，Copilot CLI 会自动压缩。想自行选择压缩时机时可使用 `/compact`；切换到无关任务并希望启动新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，Copilot CLI 允许查看并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队共享经验可以帮助所有人更好地使用 AI 工具。`/share` 可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享会话，请选择适合的目标，例如使用 `/share file` 将会话导出为本地 Markdown 文件。
3. 将导出内容发送给他人或发布前，先审查内容。会话导出可能包含提示词、回复和项目信息。

发布链接或 gist 是可选操作。不要公开团队不希望共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会流程已完成。Copilot CLI 还提供用于处理较大任务的命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行运行。
- `/worktree` 可为独立任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 GitHub Copilot 云智能体，由其异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程任务。尝试前先启动新的、范围明确的任务，并通过正常工作流程审查结果。要深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

你已完成以下工作：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 浏览可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

还有更多斜杠命令和 Copilot CLI 功能可以探索。接下来，[回顾所学内容][next-lesson]并了解后续步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
