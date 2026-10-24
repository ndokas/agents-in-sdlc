---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专用人工智能（AI）助手。可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，并为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识。团队可以将经验整理为可复用智能体，例如遵循 [Web Content Accessibility Guidelines（WCAG）][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或确保测试模式一致的测试智能体。

项目级自定义智能体定义在 `.github/agents` 文件夹中，也可以在 `~/.copilot/agents` 中全局定义。每个文件都有 YAML front matter，至少包含 `name` 和 `description`，后面是用于定义智能体行为、专长和说明的 Markdown 提示词。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体有自己的上下文窗口，并可在工作中协调使用技能，甚至其他智能体。本实验中的无障碍自定义智能体会依据无障碍准则审查并更新网站，也可以调用拉取请求工作流或运行、管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 相关的其他工作一样，需要通过测试和迭代找到适合环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将了解：

- 如何定义自定义智能体；
- 如何向自定义智能体分配任务。

## 场景

Tailspin Toys 希望让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期反馈指出，深色主题的文本与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。无障碍至关重要，因此将使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

项目已预先创建无障碍自定义智能体。先查看其内容，了解它如何引导 Copilot。

1. 返回 Codespace 并打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML front matter。

   > [!CAUTION]
   > 自定义智能体必须包含带有 `name` 和 `description` 的 front matter。

3. 浏览后续部分，了解生成无障碍网站代码时的核心职责、无障碍最佳实践、HTML/CSS/JavaScript 示例，以及常见陷阱。

## 创建并分配议题

Mission control 是管理环境中所有智能体的中心位置。可以在此分配任务、监控任务，甚至指导和调整正在进行的会话。接下来创建高对比度模式议题并分配给无障碍智能体。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。
   ![云智能体分配界面截图，其中自定义智能体列表里的 Accessibility agent 已突出显示](../../../_images/ex5-select-custom-agent.png)
10. 选择 **Assign**。
11. Copilot 开始在后台处理此任务。

## 总结与后续步骤

本练习探索了 GitHub Copilot 中的[自定义智能体][custom-agents]。自定义智能体是针对特定任务和领域的专用 AI 助手，可以将团队经验和标准整理为可复用指导。

你了解了以下内容：

- 如何定义自定义智能体；
- 如何向自定义智能体分配任务。

Copilot 正在实现高对比度模式。现在可以在 Mission Control 中[监控并指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织启用自定义智能体的准备工作][org-custom-agents]
- [为企业启用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
