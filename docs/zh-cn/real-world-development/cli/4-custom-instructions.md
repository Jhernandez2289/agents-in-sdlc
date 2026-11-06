---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "探索仓库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要特定实现方式，应确保 Copilot 能获取相关指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。筛选功能已经构建完成，现在来了解 Copilot 使用的说明、添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解仓库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码生成的影响。

## 场景

Tailspin Toys 有一套开发实践准则：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数接受可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约，相关代码更改时也应更新注释。
- 保留现有格式和 lint 指南。

通过说明文件，可以让 Copilot 按照这些实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解编码风格和需求。可以指定编码约定、库以及注释要求。说明既可以适用于整个仓库，也可以针对特定文件类型。

- `.github/copilot-instructions.md`：每次仓库请求都会发送给 Copilot 的单个说明文件，应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境支持的其他说明格式可能不同。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目的自定义说明文件

项目模板已包含核心 `copilot-instructions.md` 和若干 `*.instructions.md` 文件。先检查现有内容，再进行更改。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。仓库级说明适用于与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。该 glob 决定哪些文件适用这些说明。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他说明文件和项目现有文件的引用。

## 更新说明文件以符合团队指导

现在更新核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 将以下内容作为最后一条要点添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载仓库说明。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。确认新增的 TSDoc 注释准确说明代码。

## 总结和后续步骤

你探索了 Copilot 如何从说明文件获取上下文，并将新的标准应用于功能。具体来说，你：

- 探索了仓库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，自定义并运行可复用的 [quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持参考][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
