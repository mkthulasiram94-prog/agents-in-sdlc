---
title: "レッスン 2 - 星評価を追加する: 小さな成功"
description: "GitHub Copilot app で最初の agent session を開始し、ゲーム カードを変更して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

workspace と quick chat を確認したので、**agent session** を開始し、プロジェクトに最初の変更を加えます。ゲームのデータには星評価が含まれていますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエスト (PR) をマージします。

このレッスンでは、次のことを行います。

- agent session を開始し、その構成を確認します。
- エージェントに小さく範囲を絞った変更を依頼します。
- workspace の差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。ホーム ページのゲーム カードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。最初の session で、各カードに既存の評価を表示する小さな変更を行います。

## session の構成

**Session** は、それぞれ独立した workspace で実行されるエージェントとの会話です。各 session に専用の git worktree と branch があるため、変更を競合させずに複数の session を並行して実行できます。sidebar では repository ごとに session がまとまっています。

session では、エージェントとの**会話**、ファイルを調査・編集する**tool の動作**、差分付きの**変更ファイル一覧**を確認できます。

## session を開始して変更を依頼する

新しい session を開始してプロジェクトを確認し、機能を実装します。前のレッスンで GitHub repository からプロジェクトを app に追加しました。

1. GitHub Copilot app を開きます。
2. sidebar 上部の **+** を選択して session を開始します。
3. repository として `tailspin-toys` を選択します。
4. prompt box の下で **new working tree** と **Interactive** mode を選択し、次の prompt を送信します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して prompt を Copilot に送信します。

Copilot app は専用の worktree を作成してからプロジェクトを調べ、必要なファイルを見つけてコードを変更します。

## 差分をレビューする

人工知能 (AI) が生成した変更は、小さなものでもマージ前にレビューします。

1. アプリ右上の **Toggle review panel** を選択し、開いた panel で **Changes** を選んで変更ファイルと差分を確認します。

   ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. `GameCard.astro` に、評価がある場合の表示と `starRating` が `null` の場合の `No rating yet` 表示が追加されていることを確認します。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は確率的に動作するため、コードは上記と異なる場合があります。完全一致ではなく、同等の動作を確認してください。

## 変更を確認する

ブラウザーを開く前に、自動チェックの結果をレビューします。数値の `starRating` と `null` fallback の両方をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストール要求は内容を確認してから承認します。

Copilot に Web サイトを起動させ、ブラウザー canvas で確認します。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブな widget です。後のレッスンで custom canvas を確認して作成します。ここでは組み込みの browser canvas を使います。

1. 次の prompt を送信します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. app が起動し、Copilot app 内に browser window が開くまで待ちます。
3. 評価のあるゲーム カードに 5 段階評価が表示されることを確認します。
4. 次の prompt で、この session で起動した開発サーバーを停止します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

1. 右上の **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、認証します。
3. repository policy によって PR 作成前に関連 issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成するまで待ちます。
5. chat の上にある **PR** bubble を選択し、review pane で PR を確認します。
6. 問題がなければ **Ready to merge** を選択します。
7. 表示された dialog で **Merge pull request** を選択します。

## まとめと次のステップ

最初の agent session から変更を公開しました。具体的には、次のことを行いました。

- agent session を開始し、その構成を確認しました。
- ゲーム カードに小さく範囲を絞った変更を依頼しました。
- workspace の差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は、[絞り込み issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app で agent session を操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして設定する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
