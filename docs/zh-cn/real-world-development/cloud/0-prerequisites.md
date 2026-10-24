---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 GitHub Copilot 云智能体练习前，需要先完成环境准备。你将创建 Tailspin Toys 存储库副本，并启动用于编辑说明文件和审查云智能体工作的 [Codespace][codespaces]。

## 设置实验室存储库

先根据[模板存储库][template-repository]创建自己的副本，其中包含完成实验所需的文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建存储库。

   ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新存储库的路径（**组织或用户名/存储库名**），后续练习会用到。

## 创建 Codespace

接下来使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码，并提供支持多种编程语言、扩展和工具的集成开发环境（IDE）。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以先探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会返回 Codespace。暂时不要关闭浏览器标签页。

> [!NOTE]
> 本研讨会适用于 Codespace 或本地 [开发容器][dev-containers]。两者都已准备好实验所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**；VS Code 将构建与 Codespace 相同的开发容器。

## 总结

你已创建实验室存储库副本，并开始创建后续练习所需的 Codespace。

## 下一步

接下来，为云智能体添加它将遵循的自定义说明。继续学习[练习 1：自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [根据模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
