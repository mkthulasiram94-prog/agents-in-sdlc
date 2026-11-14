---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可用于创建专门的人工智能（AI）助手，处理开发工作流中的特定任务或领域。通过在存储库的 `.github/agents` 文件夹中定义智能体，可以为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识整理成可复用的智能体，例如遵循《Web 内容无障碍指南》（WCAG）的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面再以 Markdown 提示定义智能体的行为、专业能力和指令。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是工具**。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。在本练习中，无障碍访问自定义智能体会根据无障碍指南审查并更新网站；它可以在工作中调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要根据环境和场景反复测试、调整。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户难以阅读当前深色主题，因为文字和背景的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成相关功能。

在本练习中，你将：

- 审查现有的无障碍访问自定义智能体。
- 在 Copilot Chat 智能体模式中使用无障碍访问智能体，实现高对比度模式。

## 审查无障碍访问自定义智能体

项目已经为你创建了无障碍访问自定义智能体。先审查其内容，了解它将如何指导 Copilot。

为无障碍访问工作创建独立分支，避免与练习 3 中的筛选功能拉取请求（PR）混在一起。接下来切换回 `main`、创建分支，再查看已提供的自定义智能体。

1. 返回 Codespace 并打开终端。
2. 从 `main` 创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. 打开 `.github/agents/accessibility.md`。
4. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

5. 浏览后续章节，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍访问最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，会话将遵循该智能体的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。不同练习所用的模式和模型可能不同；需要时会在相应步骤中说明。

1. 如果 Chat 视图中尚未选择 **Agent**，请从智能体下拉菜单中选择它。

   ![Chat 视图中智能体选择器的截图，显示可选智能体列表](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部显示当前智能体（默认是 **default**）的下拉菜单。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍访问智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改方案，并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库，可能会编辑 Astro 组件、CSS 和相关测试等现有文件。

下一练习将审查并指导这项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域设计的专用 AI 助手。使用自定义智能体，可以将团队专业知识和规范整理成可复用的智能体，更有效地指导 Copilot 完成特定工作。

本课中你：

- 了解了自定义智能体的定义方式。
- 在 Copilot Chat 智能体模式中使用了自定义智能体。

接下来，[监控并指导智能体的工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一练习：使用 Playwright MCP 测试功能][previous-lesson] | [下一练习：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
