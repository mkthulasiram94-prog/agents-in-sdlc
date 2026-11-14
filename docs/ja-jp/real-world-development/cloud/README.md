---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub でタスクを割り当てると、cloud agent がバックグラウンドでリポジトリを調査して変更を加え、プルリクエストを作成します。その間、別の作業を進められます。

この演習では、cloud agent が従う custom instruction を追加し、GitHub issue を割り当てて実装を依頼します。custom agent を確認して使い、agents dashboard からセッションの進捗を監視して案内し、作成されたプルリクエストをレビューして結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. custom instruction][ex1] | コンテキスト | cloud agent が使う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. custom agent][ex3] | 専門エージェント | custom agent を確認して使う |
| [4. エージェントの管理][ex4] | 監視 | agent セッションを監視して案内する |
| [5. 反復][ex5] | レビュー | プルリクエスト (PR) をレビューして Copilot の作業を改善し、次の手順を選ぶ |

## 前提条件

このワークショップを始める前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、Model Context Protocol (MCP)、code review、Copilot CLI の機能が含まれています。

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この harness では Model Context Protocol (MCP) を扱いません。Copilot で MCP server を使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/) を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
