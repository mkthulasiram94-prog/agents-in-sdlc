---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三个独立任务，用于改进用户体验和添加功能。云智能体能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有拥有未关闭拉取请求（PR）的智能体任务、了解执行的操作，甚至指导正在进行的会话。

在本课中，你将：

- 浏览智能体页面，监控云智能体任务。
- 指导正在进行的会话，为其添加更多功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以顺便添加浅色模式。网站已经在更新样式和添加切换功能，加入这项功能很合理。现在需要指导智能体，让它同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的应用程序编程接口（API）。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建环境。
    - 设置模型上下文协议（MCP）服务器。
    - 执行完成任务所需的各项操作。

> [!NOTE]
> 具体步骤会根据 Copilot 的工作状态和执行方式而有所不同。

4. 同时留意右侧显示的 PR 面板。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 指导云智能体

现在你已了解活动任务的状态，可以在智能体实现高对比度模式时，让它同时添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 监视会话几分钟，直到它显示设置已完成并开始执行任务。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![突出显示 Steer active session while Copilot is working 对话框的智能体页面任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。完成发送提示时正在执行的具体步骤后，它会将新请求加入工作流程。

这可能需要几分钟。等待期间，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云智能体任务的中心位置。通过任务控制中心，你可以跟踪所有活动和已完成的任务、审查正在执行的工作，并重定向进行中的任务来调整范围或补充指导。

本课中你：

- 探索了任务控制中心和智能体页面，以监控云智能体任务。
- 重定向了正在进行的会话，请求添加更多功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一练习：自定义智能体][previous-lesson] | [下一练习：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
