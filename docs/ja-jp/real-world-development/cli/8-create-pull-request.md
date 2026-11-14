---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み、instruction、skill 更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込みの実装、instruction の更新、skill の更新、quality assurance (QA) profile、テストは同じブランチに保存されています。まとめてレビューし、プルリクエスト (PR) を作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、instruction、skill、QA agent を複数の PR に分けます。このワークショップでは、絞り込みと品質確認の作業を 1 つの conversation と branch で続け、1 件の PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge とマージ ライフサイクルの自動化について学びます。
- 機能変更全体と検証結果を確認します。
- 絞り込み用 PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能を計画、実装、検証してきました。Tailspin Toys では、マージを許可する権限は開発者が保持したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために残っている作業を自動化します。有効にすると、Copilot は PR を妨げている要因 (失敗した継続的インテグレーション (CI) check の修正、レビュー コメントへの対応、必要に応じた rebase など) に対処し、リポジトリで許可されていれば GitHub の auto-merge を有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業内容を確認し、準備ができていることを判断してください。

## Agent Merge で PR を管理する

コードを作成したら、まとめてレビューし、PR を作成してから Agent Merge に残りの作業を任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent dialog を開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` コマンドを使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーション (CI) を監視します。すべて成功すると、マージを実行します。
7. Copilot が、たとえば「PR #14 was squash-merged successfully.」のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Agent Merge とマージ ライフサイクルを自動化する方法を学びました。
- 機能変更全体と検証結果を確認しました。
- 絞り込み用 PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、[Copilot CLI のスラッシュ コマンド][next-lesson]でコンテキスト、モデル、共有、cloud delegation を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
