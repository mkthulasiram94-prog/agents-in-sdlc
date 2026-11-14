---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み、instruction、skill 更新、QA profile、テストをまとめてレビューし、Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

絞り込みの実装、instruction の更新、skill の更新、quality assurance (QA) profile、テストは同じ branch に保存されています。まとめてレビューし、pull request を作成します。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、instruction、skill、QA agent を複数の PR に分けます。ワークショップでは、絞り込みと品質の作業を 1 つの session と branch で進め、1 件の PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認します。
- 機能 PR 全体と検証結果を確認します。
- レビュー後に Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

絞り込みのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、PR の残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするために残っている作業を自動化します。有効にすると、session が PR を読み取り、失敗した continuous integration (CI) check の修正、レビュー コメントへの対応、必要な場合の rebase など、マージを妨げている要因に対処します。GitHub が許可するとすぐにマージします。バックグラウンドで実行され、app を再起動しても継続し、PR がマージされると自動的に停止します。

これまでは自分で **Merge pull request** を選択していました。Agent Merge はその作業を引き受けますが、コードの編集とマージには開発者の明示的な許可が必要です。許可する操作と作業内容を確認してから、マージ権限を与えてください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したら、Agent Merge に PR の処理を任せます。

1. agent picker で **Default agent** を選択します。
2. **Create PR** の横にある dropdown を選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択して処理を開始します。dropdown の項目を選んだだけでは開始されません。

Agent Merge は次の作業を行います。

- タイトルと説明を付けて PR を作成します。
- issue から session を開始した場合は、説明文で関連 issue に言及します。
- target branch との rebase や merge conflict に対処します。
- CI を監視し、すべての check が成功することを確認します。
- 他の開発者や Copilot code review からの feedback を確認し、comment を解決する変更を加えます。
- 必要に応じて、すべて成功した後に PR を自動でマージします。

すべての check が成功した後に Agent Merge が PR をマージするよう許可します。

5. マージを許可する前に、PR の diff と検証結果をレビューします。
6. **Agent Merge** の横にある dropdown を選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次の prompt で、明示的にマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 進捗を確認し、報告されたブロッカーに対処します。次に進む前に PR の状態が **Merged** であることを確認します。merge permission が有効になったことや agent が動作中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理を自動化しました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認しました。
- 機能 PR 全体と検証結果をレビューしました。
- レビュー後に Agent Merge を許可し、PR がマージされたことを確認しました。

次は、[既存の Canvas を使い、トリアージ Canvas を作成します][next-lesson]。agent と一緒に作業を確認、計画、視覚化する方法を試します。

## リソース

- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
