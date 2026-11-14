---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一练习：先决条件][previous-lesson] · [下一练习：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]用于提供指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

在本练习中，你将：

- 探索项目专属上下文、编码指南和文档规范如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）工作环境不同，本练习不会使用提示进行前后对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。后续练习中审查云智能体创建的拉取请求时，你会看到指令文件如何影响结果。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应使用 TypeScript 文档注释（TSDoc）记录代码。
- 每个文件开头都应添加注释块，说明该文件的用途。

通过指令文件，为 Copilot 提供正确的信息，使它能按上述规范执行任务。

### 自定义指令

自定义指令可以提供上下文和偏好，帮助 Copilot 更好地理解编码风格和要求。你可以借此引导 Copilot 提供更合适的建议和代码片段。指令中可以指定首选编码规范、库，甚至注释风格。你可以为整个存储库创建指令，也可以为特定文件类型创建指令，为任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随存储库中的**每个**请求发送给 Copilot。此文件应包含适用于大多数 Copilot Chat 或命令行界面（CLI）请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型提供指南，例如 TypeScript、Astro、用户界面（UI）组件或新的单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不会全面介绍如何创建指令文件，但示例项目展示了一种典型做法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目概况、项目结构和全局编码规范。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码正例和反例。

指令文件没有唯一正确的写法，就像 AI 的使用方法也不止一种。通过实践可以找出最适合项目的方式。

> [!TIP]
> 使用 GitHub Copilot 的项目都应建立完善的指令文件集合。浏览项目中的文件时，你会发现其中包括多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本研讨会会在相关课程中介绍对应操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间查看存储库附带的指令文件，包括核心 `copilot-instructions.md` 和针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。注意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定指令适用的文件。这里会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意此项目中有关创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件底部。留意指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并提供可供 Copilot 参考的示例。此处路径相对于指令文件，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头规范。

## 添加存储库规范

接下来添加一项项目级规范：代码中应使用 TSDoc 文档注释，并在文件开头添加注释头。云智能体稍后处理你分配的议题时会读取该规范。

开始编辑前，先创建用于工作的分支。云智能体会读取议题目标分支中的指令；即使你最终会合并更改，也要先将修改提交到分支：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来记录项目编码规范，改善 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意其中记录了项目编码规范，但尚未包含代码文档要求，这也是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前所述，可以在存储库级文件（`.github/copilot-instructions.md`）中设置全局指导，也可以通过 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。项目级文件适合记录刚添加的文档注释规范等全局要求。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习将从 `main` 分支创建云智能体分支，因此必须先将指令更新合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。下一练习将议题分配给 Copilot 时，云智能体即可读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何从项目指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级规范。后续练习中，云智能体创建的拉取请求会体现该规范。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
