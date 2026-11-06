---
title: "练习 2 - 使用 Copilot Agent 模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到相关文件、进行修改、运行测试、调试，再确认是否遗漏了其他文件。Copilot Agent 模式可以自主探索项目、协调修改、运行测试并修复发现的问题。

> [!NOTE]
> Agent 模式和 cloud agent 名称相似，但适用于不同的工作方式。Agent 模式在 IDE 中执行任务，支持快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot Agent 模式探索项目、识别相关文件并协调更改。
- 使用 Agent 模式跨 UI 和数据层实现功能。
- 在将更改合并到代码库前审查 Copilot 生成的代码和测试。

## 场景

游戏列表不断增长，需要让用户按类别和发行商筛选。上一练习已添加 publishers helper，现在使用 Copilot Agent 模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改网站前，先了解当前功能。Tailspin Toys 是开发者主题棋盘游戏的众筹平台，可浏览游戏列表和详情。网站是单个 Astro 应用，在构建时生成静态 HTML。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 命令，用于启动单个 Astro 开发服务器。

1. 返回 Codespace。上一练习完成后，当前应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下地址：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示要求打开 `http://localhost:4321`，选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 开发服务器的私有隧道。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后可查看标题、描述、发行商和类别等详情。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所需的模式和模型不同，步骤中会注明。

项目模板已自动创建议题待办事项。先让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写工作坊时使用的是 Claude Sonnet 4.5，因此步骤中列出了该模型，以便尽量保持体验一致。由于大型语言模型具有概率性，你可能会看到不同建议，这是正常现象。

> [!NOTE]
> Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，允许运行列出所有议题的命令。
5. 审查生成的议题列表。

## 查看说明文件

生成代码前，先查看将用于提供项目上下文的说明文件。`.github/instructions/ui.instructions.md` 包含网站功能开发指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括：
   - 项目架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等特定文件类型说明的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分复杂指导，并提供示例。

## 实现筛选功能

完成筛选至少需要更新数据层逻辑、测试和游戏列表页 UI。让 Copilot Agent 模式处理这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用待办事项中的相关议题提示 Copilot：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位数据层、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 可能会探索项目、修改文件并运行测试，这可能需要几分钟。工作期间，编辑器中可能会显示 **Keep** 和 **Undo**；无需在工作尚未完成时选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，或暂时出现测试失败。Copilot 会在代码生成和测试之间迭代，直到不再发现错误。
9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

你使用 GitHub Copilot Agent 模式为 Tailspin Toys 网站添加了功能。你了解了如何：

- 使用 Agent 模式跨 UI 和数据层实现功能。
- 让 Copilot 探索项目、识别相关文件并协调更改。
- 在合并到代码库前审查 Copilot 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为更改创建拉取请求。

### 扩展练习：实现分页

随着游戏列表不断增长，需要添加分页。使用本练习中学到的方法，让 Copilot 更新网站以实现分页。考虑沿用现有最佳实践和说明文件、是否让用户选择每页大小，以及如何编写提示。你可能需要通过补充上下文和要求更改来迭代 Copilot；这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的 Agent 模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
