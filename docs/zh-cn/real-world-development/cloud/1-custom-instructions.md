---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就要确保这些信息可用。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明和按路径限定范围的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全仓库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会比较更新前后的提示。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时并排展示说明的影响。你将在后续审查它创建的拉取请求时看到这些说明的作用。

## 说明文件

### 场景

Tailspin Toys 的开发实践包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应添加说明该文件用途的注释块。

### 自定义说明

自定义说明可以提供上下文和偏好，帮助 Copilot 理解编码风格和需求。可以指定编码约定、库以及注释要求。说明既可以适用于整个仓库，也可以针对特定文件类型。

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的仓库级说明文件，应包含适用于大多数请求的项目信息。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 不同环境支持的其他说明格式可能不同。查阅[自定义说明支持参考][custom-instructions-support]后再依赖特定格式。

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

先查看仓库附带的说明文件，之后再观察它们的作用。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还包含 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。该 glob 决定哪些文件适用这些说明，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中为本项目编写单元测试的专门说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他说明文件和项目现有文件的链接。较大的说明集可以拆分为可复用文件并链接示例；路径相对于说明文件所在位置，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的仓库标准

现在添加全仓库规则，要求代码使用 TSDoc 文档注释和文件级注释头。Cloud agent 会在之后处理分配给它的议题时读取这些说明。

1. 在 Codespace 终端中创建并切换到工作分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`。
3. 找到 **Code formatting requirements** 部分，约在第 27 行。注意当前规则尚未要求代码内文档。
4. 在现有标准下方添加以下内容：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. 保存 `copilot-instructions.md`。

> [!TIP]
> 仓库级文件 `.github/copilot-instructions.md` 适合记录全局标准；`*.instructions.md` 文件则适合特定语言、文件类型或任务。

## 提交、推送并合并说明更新

Cloud agent 会从议题目标分支读取说明。下一练习中，分配议题后它会从 `main` 创建分支，因此必须先将此说明更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理议题时即可读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成工作坊，可以跳过创建分支并直接提交到 `main`。此处创建分支是为了模拟真实项目工作流。

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了全仓库标准。后续可以在 cloud agent 创建的拉取请求中观察该标准的作用。

接下来，将第一个议题[分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
