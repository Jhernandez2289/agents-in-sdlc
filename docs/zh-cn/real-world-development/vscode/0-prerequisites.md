---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，先准备好所需环境。你将创建 Tailspin Toys 仓库副本、启动用于练习的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 正常运行。

## 设置练习仓库

从[模板仓库][template-repository]创建自己的项目副本。它包含本练习所需的文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习仓库页面选择 **Use this template**，再选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果参加 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建仓库。

   ![填写仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的仓库路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建仓库时，GitHub 会自动为你创建一组议题。整个工作坊都将使用这些议题，无需自行创建。

## 创建 Codespace

接下来，使用 Codespace 完成本练习。

[GitHub Codespaces][codespaces] 是云端开发环境，可直接在浏览器中编写、运行和调试代码，并支持多种编程语言、扩展和工具。

1. 打开刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建 Codespace。

   ![创建 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建需要几分钟，但比手动安装所有服务快得多。等待期间，可以先了解 GitHub Copilot 的其他功能。

> [!CAUTION]
> 之后还会返回该 Codespace。现在先让它保持打开状态。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都会准备好所需环境。如果要在本地运行，请在 VS Code 中打开克隆的仓库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和 Agent 模式

要在 VS Code 中使用 GitHub Copilot Chat Agent 模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespaces 时，该扩展通常已安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 的扩展视图中搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证。

1. 返回 Codespace。
2. 如果编辑器右侧没有 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入 `Hello world` 并按 <kbd>Enter</kbd>，以启动 Copilot Chat。
4. 如果尚未完成身份验证，系统会提示登录 GitHub 账户。按提示完成身份验证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示。

## 总结

你创建了练习仓库副本并开始创建 Codespace，之后将在其中编写代码。

## 后续步骤

接下来开始使用 Copilot。继续[练习 1：自定义说明][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
