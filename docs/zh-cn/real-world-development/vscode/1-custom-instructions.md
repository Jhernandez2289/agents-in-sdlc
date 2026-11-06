---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent 模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就要确保这些信息可用。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要生成什么代码，也了解代码应如何编写。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明和按路径限定范围的说明文件提供给 Copilot。
- 在当前说明下生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加新的全仓库标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选代码切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能无法完全符合设定的标准。Copilot 的结果具有概率性。本练习的重点是观察更新说明后行为的整体变化，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 的开发实践包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应添加说明该文件用途的注释块。

通过说明文件，可以让 Copilot 按照这些实践完成任务。

### 自定义说明

自定义说明可以提供上下文和偏好，帮助 Copilot 理解编码风格和需求。可以指定编码约定、库以及注释要求。说明既可以适用于整个仓库，也可以针对特定文件类型。

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的仓库级说明文件，应包含适用于大多数请求的项目信息。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不会用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用仓库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 还支持其他标准的说明文件，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 用于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型或任务提供详细指导。
- 使用清晰的自然语言，并提供代码应如何编写或不应如何编写的示例。

没有唯一正确的说明文件组织方式；可以通过试验找到适合项目的方法。

> [!TIP]
> GitHub Copilot 项目可以维护一组完善的说明文件，例如 [UI 更新说明][ui-instructions]和 [Astro 说明][astro-instructions]。Copilot 也能帮助生成说明文件：VS Code 中可选择 **Configure Chat → Generate Agent Instructions**，Copilot CLI 中可使用 `/init`。还可以从 [awesome-copilot][awesome-copilot] 获取模板和示例。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目的自定义说明文件

先查看仓库附带的说明文件，了解 Copilot 将使用的上下文。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还包含 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。它使用相对于仓库根目录的 glob 来确定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中为本项目编写单元测试的专门说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他说明文件和项目现有文件的链接。较大的说明集可以拆分为可复用文件，也可以链接示例供 Copilot 参考。链接路径相对于说明文件所在位置，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

先为后续更改创建分支。

1. 返回上一个练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所需的模式和模型不同，步骤中会注明。

先使用当前说明文件发送提示，观察生成结果；稍后更新说明并再次发送相同提示。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，为 helper 提供上下文。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常还会修改 helper 测试。
6. 注意生成的 helper 是带类型的函数，接收 `db` 客户端并返回带类型的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性；即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，重点仍是观察添加说明后的*一致性*变化。

## 添加新的仓库标准

现在更新 `.github/copilot-instructions.md`，记录仓库编码标准并改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意该部分尚未包含代码文档要求。
3. 在现有标准下方添加以下内容，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 仓库级文件 `.github/copilot-instructions.md` 适合记录全局标准；`*.instructions.md` 文件则适合特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话上下文。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 聚焦到该文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 确认生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot 现在已按新的项目标准更新代码，下一练习会继续使用这部分代码。

## 提交说明和推送分支

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新及 helper 基础代码；如果状态中显示测试等其他配套文件，也要一并暂存：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的文档标准议题。

> [!NOTE]
> 这是 Copilot 首次代表你执行 GitHub 操作。项目模板已配置 GitHub 工具。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub 并授权。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 要求 Copilot 找到与说明文件更新有关的议题，并从当前分支创建 PR，说明 PR 包含说明更新和 publishers helper 基础代码：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 提示 Copilot 代表你执行 GitHub 操作时，先审查操作内容，再酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 将其合并并把本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 提示 Copilot 执行 GitHub 操作和 shell 命令时，先审查命令，再酌情选择 **Allow**。

## 总结和后续步骤

你探索了 Copilot 如何从说明文件获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，并观察当前说明下生成的内容。
- 向 `.github/copilot-instructions.md` 添加全仓库标准。
- 再次发送相同提示，观察代码如何采用新标准。
- 将说明更新和 helper 基础代码提交并合并到 `main`。

接下来，在 [Agent 模式][next-lesson]中运用这些说明，让 Copilot 跨文件实现新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
