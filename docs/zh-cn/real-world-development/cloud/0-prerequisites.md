---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，先准备好所需环境。你将创建 Tailspin Toys 仓库副本并启动 [Codespace][codespaces]，用于编辑说明文件和审查 cloud agent 的工作。

## 设置练习仓库

从[模板仓库][template-repository]创建自己的代码仓库副本。它包含本练习所需的文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习仓库页面选择 **Use this template**，再选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果参加 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建仓库。

   ![填写仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的仓库路径（**organization-or-user-name/repository-name**），稍后会用到。

## 创建 Codespace

接下来，使用 Codespace 完成本练习。

[GitHub Codespaces][codespaces] 是云端开发环境，可直接在浏览器中编写、运行和调试代码，并支持多种编程语言、扩展和工具。

1. 打开刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

   ![创建 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建需要几分钟，但比手动安装所有服务快得多。等待期间，可以了解 GitHub Copilot 的其他功能。

> [!CAUTION]
> 之后还会返回该 Codespace。现在先让它保持打开状态。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都会准备好所需环境。如果要在本地运行，请在 VS Code 中打开克隆的仓库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

你已创建练习仓库副本，并开始创建 Codespace；后续会在其中与 Copilot cloud agent 协作。

## 后续步骤

接下来，添加 cloud agent 会遵循的自定义说明。继续[练习 1：自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
