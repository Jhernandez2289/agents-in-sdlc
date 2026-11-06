---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent 模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用 Agent 模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。接下来连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建拉取请求。

在本练习中，你将：

- 了解模型上下文协议（MCP）如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

Agent 模式可以通过外部工具扩展能力。模型上下文协议（MCP）是 Copilot 与外部工具和服务通信的标准方式。

![Agent 模式如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让 AI 智能体能够与外部工具和服务通信。智能体可以通过资源获取最新信息，并通过工具代表你执行操作。MCP 服务器充当智能体与外部服务之间的桥梁，每个服务器提供一组工具和资源。

常见服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化能力，例如访问网页、填写表单和选择按钮。

GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审查 MCP 服务器。使用前检查源代码、确认发布者并评估安全影响。只使用信任的服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 确认其中已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

该 `github` 配置来自项目模板，因此 Copilot 能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，首次使用工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 配置：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程的 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数让 Playwright 在后台运行浏览器，适用于没有桌面环境的 Codespace。

> [!NOTE]
> Tailspin Toys 已使用 Playwright 运行端到端测试，因此通常已安装浏览器。如果 Copilot 报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会询问是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，选择 `playwright` 配置上方显示的 **Start** 操作，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的；若跳过，VS Code 会在下一步首次需要工具时启动服务器并询问是否信任。

## 测试筛选功能

连接服务器并启动应用后，让 Copilot 操作浏览器并检查刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Playwright MCP 服务器会启动浏览器、执行这些步骤并报告结果。对照议题中的验收标准审查报告；如果发现异常，请继续询问或要求 Copilot 修复代码，再创建 PR。

Playwright MCP 服务器基于页面的[无障碍树][playwright-mcp-server]工作，而不是截图。这意味着智能体会处理带标签的结构化元素（按钮、链接和列表项），与辅助技术类似，因此功能检查也能初步检查无障碍性。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果提示缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支并创建拉取请求（PR），供团队审查。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改文件的 Source Control 面板](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

也可以通过 github.com 或 GitHub 命令行接口（CLI）创建 PR；这里让 Copilot 查找相关议题并关联 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示进行身份验证。
5. 确认 Copilot 找到了正确的议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但暂时**不要合并**。

## 总结和后续步骤

在本练习中，你：

- 了解了模型上下文协议（MCP）如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前使用它通过浏览器测试筛选功能。
- 发布分支并创建了筛选功能 PR。

你使用 MCP 服务器测试了功能；MCP 也可以用其他方式扩展 Copilot。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
