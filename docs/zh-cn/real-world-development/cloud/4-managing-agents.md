---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义智能体][previous-lesson]

前几项练习中，你让 Copilot cloud agent 处理了三项改进用户体验和添加功能的任务。cloud agent 虽然异步自主工作，但监控任务仍然很重要。

可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents 页面][agents-page]。在这个控制中心，可以查看所有有未关闭拉取请求（PR）的智能体任务、查看它们执行的操作，并在会话进行中引导工作。

在本练习中，你将：

- 使用 agents 页面监控 cloud agent 任务。
- 在会话进行中要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加模式切换功能，可以引导它一并实现浅色模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表应包含已分配的任务，例如：
   - 更新代码库文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如：
   - 从仓库检出代码。
   - 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的步骤会根据任务状态和 Copilot 采用的方案而变化。

4. 查看右侧显示的 PR 面板，以监控 PR 和更改文件。

## 引导 cloud agent

查看正在运行的任务后，要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与高对比度模式有关的会话。会话标题可能因 Copilot 采用的名称和任务状态而异。

   ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents 页面中突出显示 Steer active session while Copilot is working 对话框的 cloud agent 任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 如何确认提示并将其加入工作流程。

## 让 Copilot 继续工作

Copilot 会在完成当前步骤后，将新要求纳入任务。此过程可能需要几分钟，可以利用这段时间回顾已学内容。

## 总结和后续步骤

本练习介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪正在进行和已完成的任务、审查工作，也可以在会话进行中调整范围或补充指导。

你了解了：

- 如何使用 agents 页面监控 cloud agent 任务。
- 如何在会话进行中要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
