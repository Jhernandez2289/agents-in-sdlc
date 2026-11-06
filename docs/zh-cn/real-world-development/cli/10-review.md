---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并首个拉取请求（PR）。
- 根据筛选议题制定方案，在 Plan 模式中规划、使用 Autopilot 模式构建，并在 Interactive 模式中审查。
- 使用自定义说明指导智能体，然后自定义现有 `quality-checks` 技能并运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的更改

本工作坊包含两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片中显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化工作坊，这些工作合并到一个 PR 中。

## 多种验证方式

你通过自动化测试、自己在浏览器中的检查，以及 Copilot 使用 MCP 进行的浏览器探索来验证代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求和测试覆盖率审查结合起来，再交由 PR 审查。

新增测试应解决实际覆盖缺口；如果 QA 检查后认为无需新增测试，也可能是合理结果。合并前审查代码和证据；更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在其他对话中复用这些自定义项，并根据团队需求调整。说明定义标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常修改可选择更快的模型；复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响输出。

## 深入探索

核心工作流已完成，还可以了解一些其他 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务交给 Copilot cloud agent。

## 后续步骤

想要熟练使用工具，最好的办法就是持续实践。可以将其用于生产代码、个人项目，或一直想做却尚未动手的小应用。与团队分享经验，也向团队成员学习，并继续探索相关文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
