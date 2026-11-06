---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：管理智能体][previous-lesson]

## 审查工作

在本工作坊中，你让 GitHub Copilot 处理了多个改善用户体验和添加功能的任务：为代码添加文档、构建相关游戏功能，以及实现高对比度和浅色模式。现在审查代码更改，必要时向 Copilot 提供反馈。

### 场景

使用生成式 AI 不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码并遵循常规 DevOps 流程。现在先审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，再开放团队审查。

## GitHub Copilot cloud agent 安全性

Copilot cloud agent 会异步且自主地执行任务，因此受到以下安全限制：

- Copilot 只有仓库读取权限，并且只能写入它用于提交代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并使用单独的临时环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受限][agent-firewall]，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，查看 Copilot cloud agent 创建的第一个文档 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为 Copilot 默认只能有限访问外部资源，包括 MCP 服务器。可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 查看拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待它完成；这可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 注释和其他文档。具体更改可能不同。检查 TypeScript 约定和文件注释头；它们来自练习 2 中查看的自定义说明文件。
6. 审查满意后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 确认工作流已加入 PR 的检查列表。单个 Astro 应用的项目检查应通过；这可能需要几分钟。

## 要求 GitHub Copilot 修改

你可以像在 PR 中评论其他团队成员一样，提及 Copilot 并要求它修改工作。Copilot 会读取评论并启动新会话处理请求。由于结果具有概率性，无法规定唯一的提示；可以要求 Copilot：

- 在每个代码文件开头添加简短的用途说明注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 在 PR 中添加评论并提及 **@copilot**，要求它修改生成的文档。可以使用上面的建议，也可以提出其他改进要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话进行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，确认 PR 中出现了新的提交。
5. 选择 **Files changed** 选项卡并审查修改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为可审查状态，再合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查满意后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 确认工作流已加入 PR 的检查列表，并等待单个 Astro 应用的项目检查通过。
9. **可选：** 在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。你可以继续通过在 **Conversation** 选项卡中添加评论来要求它迭代，例如调整显示的相关游戏数量，或补充文件注释头和 TSDoc 文档注释。注意，这项任务是在自定义说明更新之前分配的。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的功能。PR 应包含练习 3 中要求的高对比度模式，以及练习 4 中补充的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待它完成。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查满意后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 确认工作流已加入 PR 的检查列表，并等待单个 Astro 应用的项目检查通过。
10. **可选：** 在 Codespace 中切换到 Copilot 创建的分支，手动测试无障碍功能：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍保留设置。

无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理前面 PR 一样在评论中提及 **@copilot** 并要求更新。

## 可选练习：继续委派工作

Cloud agent 很适合处理真实待办事项，让你专注于其他工作。可以在自己的仓库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你完成了 cloud agent 工作流程。在这些课程中，你：

- **检查了仓库提供的自定义说明文件**，以便之后了解它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它异步设置环境、规划和执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为控制中心**，监控并引导无障碍会话。
- **审查并迭代 cloud agent 的拉取请求**，通过提及 `@copilot` 要求更改并批准工作流。

## 回顾和后续步骤

Cloud agent 工作流程已完成。其他工作环境可以补充本次实践：

- 🖥️ **[VS Code 工作环境](../../vscode/)**：在 IDE 中探索 Copilot Chat Agent 模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)**：从终端交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

在自己的仓库中还可以尝试：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
