---
title: "第 8 课 - 创建并合并功能拉取请求（PR）"
description: "一并审查筛选功能、指令、技能更新、质量保证（QA）配置文件和测试，然后创建拉取请求并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一个分支中。现在应一并审查这些内容并创建拉取请求。你已经亲自合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge（智能体合并）**处理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化研讨会流程，筛选功能和质量工作流一直使用同一个对话和分支，所有更改都会放入这个 PR。

在本课中，你将：

- 了解什么是 Agent Merge，以及它如何自动处理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 完成审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

之前都是由你亲自选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用前，请先审查更改并确认其已准备好合并。

## 使用 Agent Merge 处理 PR

代码全部创建后，一起审查代码并创建 PR，然后让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 发出的类似消息：“PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。在本课中，你：

- 了解了 Agent Merge 及其自动处理合并生命周期的方式。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 完成审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
