---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。可以通过多种方式向 Copilot 提供特定上下文，其中[说明文件][instruction-files]是指导代码生成的重要方式。

本练习将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 在现有说明生效时生成筛选功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准；
- 重新运行相同提示词，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能基础提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合设定的标准。Copilot 具有非确定性。本练习的目标是观察更新说明后的行为趋势，而不是让输出逐字符匹配。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求：数据层需要单元测试；UI 应采用现代深色模式；代码应包含 TSDoc 文档注释；每个文件开头应有说明文件用途的注释块。使用说明文件可帮助 Copilot 按这些实践要求完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它了解编码风格和需求。可以指定编码约定、库以及注释要求。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

- `.github/copilot-instructions.md` 是存储库级说明文件，会随该存储库的**每个**请求发送给 Copilot。可包含项目技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供说明，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时都会使用存储库级说明，以及带 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

- 在 `copilot-instructions.md` 中集中记录项目级指导，例如项目结构和全局编码标准。
- 用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指导。
- 使用清晰的自然语言，并提供代码应该和不应该如何编写的示例。

关于如何创建说明文件的完整讨论超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

说明文件没有唯一正确的编写方式，使用人工智能也没有唯一正确的方法。通过试验找到适合项目的做法。

> [!TIP]
> 使用 GitHub Copilot 的项目应有完善的说明文件。此项目中包含针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的说明。Copilot 也可以帮助生成说明文件；各个产品界面的操作方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`），相关练习会在适用时说明当前界面的操作。还可以从 [awesome-copilot][awesome-copilot] 查找说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

先浏览存储库已有的核心 `copilot-instructions.md` 和面向不同任务的 `*.instructions.md` 文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹，浏览 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 指定适用文件，例如 `**/*.test.ts`。
5. 查看其中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部。该文件链接到其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件，便于拆分并复用较大的说明集，也能为 Copilot 提供示例。路径相对于说明文件本身，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 创建分支

先从上一个练习返回 Codespace，然后创建工作分支。

1. 返回 Codespace。
2. 打开终端：在 Mac 上按 <kbd>Control</kbd>+<kbd>`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>`</kbd>。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

先使用当前说明生成代码，之后更新说明并重新发送相同提示词，以便观察差异。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始干净的对话。各练习的模式和模型选择可能不同，相关步骤会注明。

1. 关闭之前练习留下的编辑器标签页，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. 查看 Copilot 探索项目并应用代码更改的过程，它可能会同时修改 helper 和测试文件。
6. 注意建议的 helper 是带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码可能**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，即使没有明确要求，也可能添加文档注释。即便如此也没关系；重点仍是观察更新说明后的一致性变化。

## 添加新的存储库标准

在 `.github/copilot-instructions.md` 中添加存储库级标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录了编码标准，但尚未加入代码文档要求。
3. 在现有标准下方添加以下 Markdown 行：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可以放在存储库级别（`.github/copilot-instructions.md`），用于全局指导；也可以用 `*.instructions.md` 针对语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 重新运行提示词并查看变化

现在使用更新后的说明重新生成 helper。

1. 返回 Copilot Chat 并选择 **New Chat**，清除当前对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 重新发送相同提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看建议的文件是否以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看建议的函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot 现在已遵循新的项目标准，并将其应用到后续练习会继续扩展的代码中。

## 提交并推送分支

说明文件和其他存储库资源一样，需要通过源代码管理进行提交。

1. 打开新终端：在 Mac 上按 <kbd>Control</kbd>+<kbd>`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>`</kbd>。
2. 运行以下命令确认说明更新和 helper 更改：

   ```bash
   git status
   ```

3. 暂存说明更新和 Copilot 为 helper 基础所做的更改，然后提交：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试文件），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

将分支推送后，为待办事项中的说明文档议题创建拉取请求。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用其中一个工具时，VS Code 可能会提示你登录 GitHub；请按照提示完成授权。后续练习会介绍这种连接的工作方式（模型上下文协议，MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 在 Mac 上按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>，或在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>，打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 发送以下提示词，请 Copilot 查找相关议题并从当前分支创建 PR：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 请求许可来执行 GitHub 操作，请审查命令，再按需选择 **Allow**。
6. 创建 PR 后，发送以下提示词，请 Copilot 将 PR 合并到 `main` 并更新本地分支：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果出现 GitHub 操作和 shell 命令许可请求，请先审查命令，再按需选择 **Allow**。

## 总结与后续步骤

你已了解 Copilot 如何从说明文件获取上下文，并在 VS Code 的 Copilot Chat 中使用它：

- 探索 Copilot 如何从存储库说明和按路径限定范围的说明文件获取上下文；
- 使用现有说明发送代码生成提示词并查看结果；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准；
- 重新发送相同提示词，观察建议的代码采用新标准；
- 提交说明更新和 helper 基础，并合并到 `main`，供下一练习继续使用。

接下来，使用[智能体模式][next-lesson]实现跨代码库的新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
