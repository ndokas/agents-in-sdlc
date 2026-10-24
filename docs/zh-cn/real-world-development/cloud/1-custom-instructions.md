---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 应了解某些背景信息，就需要确保这些信息可以获取。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，还了解应该如何完成。

本练习将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目上下文、编码准则和文档标准；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，这里不会运行前后对比提示词。云智能体会在 GitHub 上异步处理议题，因此难以实时并排展示说明的影响。稍后的练习中，你可以在审查云智能体创建的拉取请求时看到这些说明的效果。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件的用途。

使用说明文件可以帮助 Copilot 按这些实践要求完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它了解编码风格和需求。你可以指定编码约定、库以及注释要求。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：单个存储库级说明文件，会随该存储库的**每个**请求发送给 Copilot。可包含项目技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的说明，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时都会使用存储库级说明，以及带 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

示例项目展示了一种可行做法：

- 在 `copilot-instructions.md` 中集中记录项目级指导，例如项目结构和全局编码标准。
- 用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指导。
- 使用清晰的自然语言，并提供代码应该和不应该如何编写的示例。

说明文件没有唯一正确的编写方式。通过试验找到适合项目的做法。

> [!TIP]
> 使用 GitHub Copilot 的项目应有一组完善的说明文件。此项目中包含针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。
>
> 可以从 [awesome-copilot][awesome-copilot] 查找说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

先浏览存储库已有的核心 `copilot-instructions.md` 和面向不同任务的 `*.instructions.md` 文件，了解 Copilot 可用的指导。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导，这些信息适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，浏览 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 指定适用文件；例如 `**/*.test.ts` 会匹配 TypeScript 测试文件。
5. 查看该文件中针对本项目单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部。该文件链接到其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件，便于拆分并复用较大的说明集，也能为 Copilot 提供代码示例。此处的路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来只需编辑一次：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后分配议题时，云智能体将读取这项标准。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录了项目编码标准，但还没有代码文档要求。
4. 在现有标准下方添加以下 Markdown 行：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可以放在存储库级别（`.github/copilot-instructions.md`），用于全局指导；也可以用 `*.instructions.md` 针对语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支上的说明。下一练习中，云智能体会从 `main` 分支创建工作，因此必须先将说明更新合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了模拟真实项目的工作方式。

## 总结与后续步骤

你已完成以下工作：

- 了解 Copilot 如何从项目说明文件中获取上下文；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

后续练习中可以在云智能体生成的拉取请求里看到该标准的效果。

接下来，将[第一个议题分配给 GitHub Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
