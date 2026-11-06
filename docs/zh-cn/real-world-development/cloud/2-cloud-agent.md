---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义说明][previous-lesson]

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或一直搁置的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。工作完成后，它会创建草稿拉取请求，供开发者审查。这样可以卸下繁琐工作，加快开发流程，让开发者专注于更重要的事项。

在本练习中，你将了解 Copilot cloud agent 如何：

- 自定义代码生成环境。
- 安全地执行操作。
- 处理范围明确的议题。
- 接收分配的议题。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初创建网站的承包商没有为应用代码添加文档。团队希望先为所有导出函数添加 TSDoc 文档注释。

设计团队还希望改善游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，让用户可以继续浏览。团队暂时只需要可用于 UX 验收测试的初步实现。该任务目前尚未排在更高优先级议题之前。

这类容易被推迟的任务很适合交给 Copilot cloud agent 异步处理。开发者可以先处理其他工作，之后再回来审查结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent][copilot-agents] 可以像开发者一样在后台处理任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始工作。你可以在会话进行中或完成后查看它的工作；提议的解决方案准备好审查时，它会在拉取请求中提及你。

## 明确指令和任务范围

使用 GitHub Copilot 并不意味着可以跳过思考。即使看似简单的任务也可能有复杂细节，因此应认真考虑如何界定 cloud agent 的任务范围。像使用 AI 结对编程助手一样，分阶段工作、学习、试验并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本仓库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的指导作用。分配 cloud agent 任务前，先只读查看仓库现有说明，之后即可观察它们对结果的影响。

在 GitHub 网页界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，确认说明适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent。审查生成的 PR 时，注意 TSDoc 注释、TypeScript 约定和文件注释头；它们来自这些说明文件。

## 为 Copilot cloud agent 设置开发环境

代码任务通常需要专用环境和设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此。

Copilot cloud agent 使用 [GitHub Actions][github-actions] 执行任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义环境，让它使用所需开发工具和依赖项。项目已预先配置此工作流，以安装 Node.js、项目依赖项和端到端测试所需的浏览器，并迁移和填充单个 Astro 应用的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从仓库的 Actions 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 只授予这些步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆仓库（例如安装依赖项），需要 contents: read 权限。如果未在设置步骤中克隆仓库，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端和应用设置：Node.js（整个应用现在是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点需要注意：

- 它只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会先运行该作业。
- `workflow_dispatch` 触发器允许你在仓库的 **Actions** 选项卡中手动运行工作流，无需等待 Copilot 启动它，适合提前验证设置。

## 添加代码文档

许多项目都存在文档过时或缺失的问题，这类技术债务会降低效率、增加维护难度，也让新开发者更难加入。Copilot cloud agent 可以在后台生成文档，因此适合处理此类任务；后续练习会审查它的工作。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新页面。
12. 几分钟后，Copilot 应会在 PR 中创建待办列表。

> [!NOTE]
> 待办列表可能需要几分钟才会显示，因为 Copilot 正在创建环境、运行前面介绍的工作流、分析项目并制定方案。

13. 审查列表及 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、制定方案并创建代码。

Copilot 可能需要几分钟完成任务。cloud agent 的主要优势之一就是异步处理工作，让开发者可以转而处理其他事项。现在再分配一项任务，然后继续开发应用功能。

## 在游戏详情页添加相关游戏

使用 GitHub Copilot cloud agent 的一项优势是可以拆分工作：你处理一项任务时，智能体可以处理另一项。现在将相关游戏功能分配给它。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择空白议题模板 **Blank issue**。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像开发者一样工作，因此无需持续监控；完成后再审查即可。现在可以继续了解自定义智能体。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 Copilot 异步处理，用于解决技术债务、开发功能或协助框架迁移。

你了解了：

- 自定义代码生成环境。
- 安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

cloud agent 在后台处理任务时，接下来可以创建并使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并支持之前介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
