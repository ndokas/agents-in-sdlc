---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查其拉取请求并确认 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

通过 GitHub Pages 为 Space Quiz 提供公开访问地址。Copilot 可以准备工作流并引导部署，但你仍需负责审查更改并确认网站已上线。

本课将：

- 请求 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已位于 GitHub 存储库中，但其他人还没有简单的体验方式。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖项，就能通过公开 URL 访问。

## 请求 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中发送以下提示词：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；包含 Build website、Upload artifact 和 Deploy to Pages 步骤的成功 GitHub Actions Deploy website 运行；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的审批请求。

## 审查工作流并验证部署

审查智能体的更改，并确认部署实际成功后再结束本课。

1. 确认只有在存储库原本没有 README 时才添加 README。
2. 审查拉取请求更改，包括工作流的构建和部署步骤。
3. 只有在必需的拉取请求检查通过后才合并。
4. 跟进 GitHub Actions 部署运行，直到运行成功。
5. 打开报告的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它给出的具体说明操作。只有部署成功且网站可以正常加载后，才能将任务视为完成。

## 总结与后续步骤

Space Quiz 现已作为网站上线，并且你已亲自验证部署，而不是仅依赖智能体的报告。继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
