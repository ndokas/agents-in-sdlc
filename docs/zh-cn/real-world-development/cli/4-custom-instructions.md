---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。[说明文件][instruction-files]不仅说明想要什么代码，还说明代码应如何组织。现在筛选功能已构建完成，接下来查看 Copilot 使用的说明、添加文档标准并将其应用到代码。

本课将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 的开发实践要求包括：注释应解释意图和不明显的决策，而不是重复代码；`db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（包括适用时可注入的 `db` 参数）；可复用 Astro 组件应记录 `Props` 契约，代码更改时注释也应同步更新；并保留现有格式和 lint 指导。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它了解编码风格和需求。`.github/copilot-instructions.md` 是随存储库每个请求发送的单一说明文件，可提供项目级信息；`.github/instructions/*.instructions.md` 则针对语言、文件类型或任务提供指导。

> [!NOTE]
> 不同工作环境对其他说明格式的支持各不相同。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明

启动项目时已包含一组说明文件。先查看现有内容，再进行更改。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目说明和编码指导。这些说明适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，浏览 Astro、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md` 并查看顶部的 `applyTo` 字段，它使用 glob 指定适用文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到的其他说明和项目文件。这有助于拆分并复用说明，也能为 Copilot 提供示例。

## 根据团队指导更新说明文件

核心 `copilot-instructions.md` 还缺少 TypeScript 文档注释标准。将该标准添加到文件中。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 将以下条目添加为该部分最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后，退出并恢复筛选对话，以便在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍打开，请输入 `/exit`。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中运行：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。

3. 发送以下提示词，要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。确认新生成的 TSDoc 注释准确解释了代码。

## 总结与后续步骤

你已完成以下工作：

- 探索存储库说明和按路径限定范围的说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 查看说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
