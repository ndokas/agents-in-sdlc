---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有 quality-checks 技能、自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不仅仅是编写代码。前面已手动验证代码，并使用说明文件确保代码符合标准。测试、lint 和持续集成 (CI) 的其他环节也很重要。**智能体技能**适合处理此类任务，可以帮助 Copilot 正确执行操作。

本课将：

- 探索现有的 `quality-checks` 技能；
- 自定义技能结果格式；
- 重新加载并运行该技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用任务说明、可执行脚本和辅助资源打包起来，供智能体按需加载。技能核心是一个以技能名称命名的文件夹，其中包含 `SKILL.md`。该 Markdown 文件通过 front matter 中的名称和描述定义技能，随后介绍技能的功能和调用时机。文件夹也可以包含脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有命令。

项目的 `.github/skills` 文件夹可存放团队共享和复用的技能；也可以将技能存放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

先查看 Tailspin Toys 团队创建的 `quality-checks` 技能，了解它如何运行单元测试、lint 和类型检查。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述可帮助 Copilot 判断何时调用技能。
3. 阅读其余说明，了解技能如何引导 Copilot 运行测试和 lint。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前运行技能

技能可以直接通过 Copilot CLI 调用，也可以使用自然语言请求。现在让 Copilot 运行这三项检查。

1. 返回筛选对话，并切换到 Interactive 模式。
2. 发送以下提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

团队希望报告说明运行了哪些检查、检查是否成功，以及工具报告的具体结果。现在更新技能以生成这类报告。

1. 重新打开 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次发送与之前完全相同的提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看新报告，并与第一次的结果比较。

## 总结与后续步骤

你已完成以下工作：

- 探索现有的 `quality-checks` 技能；
- 自定义技能结果格式；
- 重新加载并运行该技能。

该更改会与筛选功能一起进入功能 PR。下一步，让 Copilot 通过 [Playwright MCP 服务器][next-lesson]直接与网站交互。

## 更多技能示例

以下社区示例可供参考，不是额外的动手任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
