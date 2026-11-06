---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域的专用 AI 助手。通过在仓库的 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专门指令、最佳实践、编码模式和领域知识。例如，无障碍智能体可以帮助遵循 [WCAG][wcag]，安全智能体可以遵循安全编码实践，测试智能体可以维护一致的测试模式。

自定义智能体定义在项目的 `.github/agents` 文件夹中，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后续 Markdown 提示词用于定义智能体行为、专长和指令。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]都主要通过 Markdown 文件定义，并告诉 AI 如何完成工作。可以将自定义智能体视为执行工作的角色，将技能视为它使用的工具。

自定义智能体有独立的上下文窗口，可以在工作中协调技能或其他智能体。本练习中的无障碍自定义智能体会根据无障碍指南审查和更新网站，并可调用拉取请求工作流或运行测试等技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要在适用的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

在本练习中，你将了解：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。用户反馈指出，当前深色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。你将使用自定义智能体尽快生成此功能。

## 检查无障碍自定义智能体

项目已经提供无障碍自定义智能体。先查看它如何指导 Copilot。

返回 Codespace，在终端中基于 `main` 创建无障碍功能分支，以便将筛选 PR 与本练习分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续内容，了解其中说明的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见错误和陷阱。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的自定义智能体。选择智能体即可让会话使用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所需的模式和模型不同，步骤中会注明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单。默认情况下，当前智能体为 **default**。
3. 从可用智能体中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会以内联差异的形式显示文件路径和更改内容。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 该过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一练习会检查并引导这项正在进行的工作。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 自定义智能体，它们是针对特定任务和领域的专用助手。团队可以将专业知识和标准编码为可复用智能体，指导 Copilot 更有效地完成工作。

你了解了：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat Agent 模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查其更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
