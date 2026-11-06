---
title: "第 7 课 - 创建并使用质量保证（QA）智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP 进行审查。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

在本课中，你将：

- 了解自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用角色。配置文件说明智能体的用途、指令和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

不同自定义项各有用途：仓库说明描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何运用这些能力检查需求并报告发现。QA 配置不会取代这些工具，也不需要另开对话。

## 创建 QA 配置

打开功能 PR 前，让 Copilot 创建可复用的 QA 配置。该配置需说明 QA 执行的检查和必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新配置：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先确认 Copilot 记录了预期 QA 流程和边界，避免它在只要求验证时改动功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 为必填项；`name` 为可选项，但添加后会提供清晰的显示名称。
3. 阅读配置指令，确认 QA 会从需求开始，遵循仓库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，也不会提交更改或创建 PR。
5. 如果缺少任何职责或边界，先让默认智能体修改配置。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置后，恢复同一筛选对话并选择 QA，让它使用已有的议题和计划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中，但仍可使用上述命令启用。

2. 使用以下提示要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成工作。
4. 阅读它提供的报告。

## 总结和后续步骤

你添加了可复用的专用角色，并审查了它的工作。在本课中，你：

- 了解了自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择了 QA 智能体，并让它根据筛选议题报告发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备就绪。接下来将它们汇总到功能 PR 中，并使用 Agent Merge [管理 PR][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
