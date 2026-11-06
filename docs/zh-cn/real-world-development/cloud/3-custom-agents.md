---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域的专用 AI 助手。通过在仓库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专门指令、最佳实践、编码模式和领域知识。例如，无障碍智能体可以帮助遵循 [WCAG][wcag]，安全智能体可以遵循安全编码实践，测试智能体可以维护一致的测试模式。

自定义智能体定义在项目的 `.github/agents` 文件夹中，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后续 Markdown 提示词用于定义智能体行为、专长和指令。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]都主要通过 Markdown 文件定义，并告诉 AI 如何完成工作。可以将自定义智能体视为执行工作的角色，将技能视为它使用的工具。

自定义智能体有独立的上下文窗口，可以在工作中协调技能或其他智能体。本练习中的无障碍智能体会根据无障碍指南审查和更新网站，并可调用拉取请求工作流或运行测试等技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。应在适用的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

在本练习中，你将了解：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。用户反馈指出，当前深色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。你将使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

项目已经提供无障碍自定义智能体。先查看它如何指导 Copilot。

1. 在 Codespace 中打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续内容，了解其中说明的：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见错误和陷阱。

## 创建并分配议题

GitHub Copilot agents 页面是管理环境中所有智能体的中心位置。可以在其中分配任务、监控任务并提供进一步指导。现在将创建高对比度模式的任务交给 Copilot。

1. 打开仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![Copilot 议题分配界面中突出显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 的[自定义智能体][custom-agents]，它们是针对特定任务和领域的专用助手。团队可以将专业知识和标准编码为可复用智能体，指导 Copilot 更有效地完成工作。

你了解了：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，从 agents 页面[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [组织使用自定义智能体的准备工作][org-custom-agents]
- [企业使用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
