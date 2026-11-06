---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的仓库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此仓库、分支和 CI 管道都可以直接使用。它适用于同时指挥多个智能体的工作流：每个智能体都在隔离工作区中运行，让你可以并行处理工作并自动执行重复任务。Node.js 已安装、项目副本已准备好，现在安装应用并连接该仓库。

在本课中，你将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 仓库将项目添加到应用。
- 熟悉工作区，包括模板创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正在采用 AI 智能体处理不断增加的待办事项。Copilot app 提供统一的工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将帮助你完成安装和连接，并熟悉如何发起有关项目的对话。

> [!NOTE]
> 必须拥有符合条件的 Copilot 计划：Copilot Free、Copilot Student 或付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。

## 安装并配置 GitHub Copilot app

GitHub Copilot app 支持 Windows、macOS 和 Linux。安装应用、完成身份验证并添加 Tailspin Toys 仓库。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按产品页面上的说明安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub** 并按提示完成身份验证。如果使用带数据驻留的 GitHub Enterprise Cloud，选择 **Use GitHub Enterprise**，并按提示输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果初始设置期间可以选择仓库，请选择从模板创建的 Tailspin Toys 仓库。也可以跳过此步骤，之后再添加项目。
6. 按提示选择主题，然后完成初始设置。
7. 在侧边栏的 **Sessions** 下查找仓库。如果已列出，则无需再次添加。
8. 如果仓库尚未连接，选择 **Sessions** 旁的 **+** 创建会话，然后选择 **Add GitHub repository**。输入先决条件练习中记录的所有者和仓库名称。
9. 确认仓库显示在侧边栏中。此布局会将 `tailspin-toys` 等仓库文件夹归在 **Sessions** 下，每个仓库下方会列出单独的会话。

## 熟悉工作区

连接项目后，先熟悉侧边栏中的几个工作区：

- **New**：启动新的 Copilot 聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成浏览和筛选拉取请求、查看 CI 状态并审查 PR。
- **Issues**：浏览和筛选议题，并从议题启动会话。
- **Automations**：保存按计划或按需运行的智能体任务，适合管理待办事项和定期维护。总结会将其作为后续步骤，而不是本工作坊中的另一项练习。
- **Customize**：添加 MCP 服务器、插件、技能和其他组件。本工作坊将用它配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需分支或工作区。首次启动聊天后会显示此区域；如果尚未显示，选择 **Sessions** 旁的 **+** 启动首个聊天。
- **Sessions**：按仓库整理文件夹和智能体会话。选择 `tailspin-toys` 等仓库可打开仓库视图，选择其下的会话可返回对应对话。每个会话都在独立工作区中运行，因此可以并行启动多个会话而不会互相冲突。下一课会启动首个会话。

随着工作坊继续进行，你会逐步了解工作区。

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作或某件事是否可行，可以向 Copilot 提问。

### 查找模板创建的待办事项

Tailspin Toys 的模板已经创建了待办事项。现在查看其中的议题。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 仓库文件夹，而不是其下的某个会话。然后在仓库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题阅读详情。每个议题也可以作为启动智能体会话的入口。完成快速的首次更改后，你将从筛选议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的仓库中的项目。若要查看其他仓库的工作项，请将那些仓库添加到应用。

## 尝试快速聊天

快速聊天适合在不创建分支或工作树的情况下提问或集思广益，是了解应用本身的便捷方式。

1. 在侧边栏中选择 **Chats** 旁的 **+** 打开新聊天。如果 **Chats** 尚未显示，请选择 **Sessions** 旁的 **+** 启动首个聊天；之后 **Chats** 会显示。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 阅读对话视图中的回复。每个会话都在独立的 git 工作树中运行，因此可以并行运行多个智能体而不发生更改冲突。可以继续对话，也可以随时开始新聊天。

## 总结和后续步骤

你已安装 GitHub Copilot app、连接项目并熟悉工作区。你学习了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 仓库添加项目。
- 熟悉工作区，并在 **Issues** 中查找模板创建的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[启动首个智能体会话][next-lesson]，让它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
