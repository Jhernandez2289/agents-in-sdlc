---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义智能体][previous-lesson]

使用 GitHub Copilot Agent 模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于工作会实时发生在本机，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以查看内联差异、接受或拒绝单项更改，也可以通过后续提示引导会话，不必离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受提议的文件编辑。
- 在会话进行中引导智能体添加浅色模式切换开关。

## 场景

[练习 4][previous-lesson]中的无障碍自定义智能体正在实现高对比度模式。工作期间，设计团队提出还需要浅色模式切换开关，以便默认主题太暗的用户使用。你将引导现有会话扩展工作，而不是从头开始新对话。

## 监控 Copilot 的进度

Copilot Chat 会显示智能体采取的每个步骤，包括探索的文件、运行的命令和提议的编辑。每项编辑都以差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，浏览聊天内容，查看工具调用、文件读取、文件编辑和终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看本次会话中智能体修改的文件。

> [!NOTE]
> 如果工具调用要求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式，并查看它的最终更改摘要。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中要求添加浅色模式切换开关。这样智能体可以保留此前修改的组件、样式方法和本地存储模式等上下文。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会检查已有更改，并提出浅色模式相关修改。
3. 像审查高对比度模式一样，逐项检查新增编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期，例如尝试重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 使用后续提示引导正在运行的会话通常比从头开始更快。可以要求调整（例如“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（例如“将浅色模式整合到现有设置面板，不要添加第三个开关”），或扩展范围（例如“为两个开关添加键盘快捷键”）。

## 审查工作集

提交前快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并检查差异。确认实现体现了自定义智能体的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat Agent 模式监控并引导正在进行的会话：检查每项文件编辑、接受或拒绝更改，并通过后续提示扩展任务，同时保留上下文。

在本课中，你了解了：

- 如何实时监控智能体工作。
- 如何接受或拒绝单项文件编辑。
- 如何通过后续提示引导会话。

接下来，在[下一课][next-lesson]中将无障碍功能作为拉取请求发布，并审查本工作坊中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 Agent 模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
