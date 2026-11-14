---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "エージェント モードを確認し、Plan で方針に合意し、Autopilot で issue から絞り込み機能を構築して、Interactive でレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

最初は小さな機能を追加しました。より大きな変更には、堅実なプロセスが必要です。GitHub Copilot app は組織の既存フローに沿って作業できます。このレッスンから数回にわたり、issue から新機能を作り、コードと動作を確認し、プロジェクトにマージするエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能開発の間は、同じ session を使います。通常はファイル種別ごとに異なる session やプルリクエスト (PR) を使いますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい agent session を開始します。
- **Plan** mode で要件を決めます。
- **Autopilot** mode で機能を実装します。
- コードをレビューします。
- browser canvas で機能を手動検証します。

この機能開発の続きでは、repository instruction を更新し、既存の `quality-checks` skill をカスタマイズし、Model Context Protocol (MCP) による検証を追加して quality assurance (QA) agent を作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの issue には機能が記されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要です。Plan mode で決定し、Autopilot に範囲の明確な実装を任せます。

## 背景

人工知能 (AI) coding agent を開発フローに導入しても、基本は変わりません。一般的なフローは次のとおりです。

1. 作業内容を記述した issue を開きます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと continuous integration が成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くは上記の流れを基本にしています。

この標準的な進め方に従うと、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビューを通ります。

## session mode

**Session mode** は、agent の自律性を決めます。prompt field の下にある dropdown からいつでも変更できます。

- **Interactive**: 開発者と agent が協力します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan**: agent が最初に計画を作成します。実装前に計画をレビューして承認します。
- **Autopilot**: agent が自律的にコードを書き、テストを実行し、入力を待たずに反復します。

Plan mode で開始して計画をレビューし、その後 Autopilot で実装します。

## issue から session を開始する

開始前に、星評価の PR がマージされ、ローカルの `main` が最新であることを確認します。

1. sidebar の **Sessions** で Tailspin Toys repository folder を選び、repository 上部の navigation で **Issues** を選択します。既存の **Allow users to filter games by category and publisher** issue を開きます。新しい issue を作成する必要はありません。
2. **New session** を選択し、更新済み `main` に基づく **new working tree** を選びます。

   ![GitHub Copilot app の Issue view で、New session ボタンを矢印で示した画面](../../../_images/app-new-session-from-issue.png)

3. issue が session に添付されていることを確認し、mode selector から **Plan** を選択します。

> [!NOTE]
> Node.js または npm が見つからない場合は、agent に `node --version` と `npm --version` を実行させます。通常の terminal で成功するのに session 内で失敗する場合は、app の PATH または version manager の設定を確認し、app を完全に終了して再起動します。失敗した操作とチェックを再試行してください。agent が見つけられないことだけを理由に npm を再インストールしないでください。

## 絞り込み機能を計画する

計画を立てることで、Copilot がコードを書く前に方針を確認できます。issue から始めたので、機能の依頼内容はすでにコンテキストに含まれています。次を送信します。

```plaintext
Build this feature.
```

Copilot の質問に回答し、計画を issue の受け入れ条件に照らし合わせます。カテゴリとパブリッシャーの絞り込み、アクセシブルな control、データアクセスの変更、テストが含まれていることを確認します。複数カテゴリの組み合わせ方や、該当するゲームがない場合の動作など、不明確な点を話し合います。

計画には、プロジェクトの既存 tool による lint、unit test、end-to-end (E2E) test、type check を含めます。計画は絞り込みの実装とテストに集中させます。quality workflow が終わるまで PR は作成しません。計画を承認する前に修正を依頼し、後の検証で参照する issue のリンクと合意事項を控えます。

## Autopilot を明示的に承認する

計画に問題がなければ **Approve and implement this plan**、または使用中の version で同等の option を選びます。承認しただけで mode が切り替わると決めつけず、表示が **Autopilot** であることを確認します。

Copilot は計画に沿って実装を進め、コードを生成してテストを実行しながら反復します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があるため、先に計画をレビューしてください。依存関係の不足や port の競合が報告された場合は、チェック完了とみなす前にセットアップの問題を解決します。停止してよいのは、自分で起動した server だけです。

## 実装をレビューして検証する

生成されたコードは、他のコードと同じようにマージ前にレビューします。

1. **Changes** を開き、絞り込みの実装とテストを確認します。
2. 複数カテゴリやパブリッシャーの組み合わせを含め、結果を issue と承認済みの追加決定に照らし合わせます。repository instruction に従っていることも確認します。
3. lint、unit test、E2E test、type check の結果を調べます。スキップされたチェックは成功ではありません。
4. 実装を受け入れる前に失敗を解消し、影響するチェックを再実行します。Playwright の E2E 設定は preview を build して配信し、ローカル server を再利用する場合があります。以前のレッスンではなく、この worktree の server をテストしていることを確認します。

## 新機能を試す

ブラウザー canvas でサイトを開き、動作を確認します。

1. 次の prompt で、app を起動して browser canvas で開くよう Copilot に依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. Copilot app 内に browser window が開くまで待ちます。
3. カテゴリを選択し、結果が一致することを確認します。filter を解除してからパブリッシャーを選択し、同様に確認します。
4. カテゴリとパブリッシャーを組み合わせ、issue と承認済みの計画に沿うことを確認します。複数カテゴリを使える場合は、合意した組み合わせも検証します。
5. 該当するゲームがない組み合わせを試して空の状態を確認します。filter を解除またはリセットし、一覧全体が戻ることを確認します。
6. 確認が終わったら、次の prompt でこの session の開発 server を停止します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 次のレッスン用に session を保持する

まだ絞り込み PR を作成またはマージしないでください。レッスン 4 から 8 まで同じ session、worktree、branch を使います。custom instruction を適用し、skill と QA の改善を追加してから、レッスン 8 でまとめた PR を作成してマージします。開発 server を停止した後、session をそのまま残します。

## まとめと次のステップ

会話 mode を使い分けて、機能を構築しレビューしました。

- GitHub issue から新しい agent session を開始しました。
- **Plan** mode で要件を決めました。
- **Autopilot** mode で機能を実装しました。
- コードをレビューしました。
- browser canvas で機能を手動検証しました。

次は、[custom instruction を使って][next-lesson]、コードが文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot app の agent session を操作する][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
