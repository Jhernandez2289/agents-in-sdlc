---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码还涉及数据、外部服务，以及让 Copilot 使用其他自动化工具。模型上下文协议（MCP）服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

在本课中，你将：

- 了解模型上下文协议（MCP）及 Copilot CLI 对它的使用方式。
- 如果 Playwright MCP 服务器尚不可用，则添加该服务器。
- 让智能体驱动浏览器，探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与界面交互。让 Copilot 使用正在开发的网站，就能像用户一样操作网站，并帮助确认更改是否符合预期。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为 AI 智能体与外部工具和服务通信提供了方式。智能体通过 MCP 服务器访问工具和资源；服务器充当智能体与外部服务之间的桥梁，每个服务器提供一组可访问的工具和资源。

常见服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审查 MCP 服务器。使用前检查源代码、确认发布者，并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，现在添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

此前你已经手动确认功能行为。现在通过 Playwright MCP 服务器让 Copilot 做同样的检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，无需在提示中指定。不过明确指出工具有助于保持结果一致。

2. 观察 Copilot 在浏览器中执行的操作。
3. 阅读报告，确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并提供报告。

## 总结和后续步骤

你通过 Copilot CLI 在真实浏览器中使用 Playwright MCP 探索了功能。在本课中，你：

- 了解了模型上下文协议（MCP）及 Copilot CLI 对它的使用方式。
- 在 Playwright MCP 尚不可用时添加了服务器。
- 让智能体驱动浏览器，探索筛选功能。

接下来，创建一个[结合技能和浏览器工具的 QA 自定义智能体][next-lesson]。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
