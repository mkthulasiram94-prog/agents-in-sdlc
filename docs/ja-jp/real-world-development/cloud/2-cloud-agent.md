---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: custom instruction][previous-lesson]

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシー コード、時間がなくて着手できない機能など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人がレビューできる draft pull request (PR) を作成します。時間のかかる作業を委任し、開発者がより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に実行する方法
- 範囲を明確にした issue の重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys では、最初のサイトを作成した開発者がドキュメントを十分に追加しておらず、すべてのドキュメントが不足しています。まず、アプリケーションのすべての export 関数に TypeScript documentation (TSDoc) doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別のゲームを表示します。完成度の高い実装ではなく、user experience (UX) の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、優先度の高い issue が他にもあります。

こうした後回しになりやすいタスクは、Copilot cloud agent に適しています。非同期で作業を任せれば、開発者は別のタスクに集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人の開発者と同様にバックグラウンドで作業します。[GitHub issue を Copilot に割り当てる][assign-issue]と、Copilot は進捗を記録する draft プルリクエストを作成し、環境を準備してタスクに取りかかります。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした issue の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作にも複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決め][cloud-agent-best-practices]、人工知能 (AI) ペア プログラマーとして段階的に進め、試し、学び、調整します。生成 AI を加えてもソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では、custom instruction と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、cloud agent に作業を割り当てる前に、リポジトリにある instruction file を読み取り専用で確認します。GitHub の Web UI で開くか、すでに起動している Codespace で開いてください。

1. `.github/copilot-instructions.md` を開き、**Code standards** の TypeScript 規約と TSDoc doc comment の要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` file に適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。後の演習で PR をレビューするとき、これらの instruction に由来する TSDoc、TypeScript 規約、コメント ヘッダーを確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成するには、通常、必要な開発 tool と依存関係を準備する必要があります。Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に [setup workflow][setup-workflow] を作成すると、作業前に必要な開発 tool と依存関係を設定できます。この workshop 用の workflow はあらかじめ用意されており、Node.js、プロジェクト依存関係、end-to-end test 用ブラウザー、単一 Astro app 用に migration と seed を済ませた SQLite database を準備します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow と似ていますが、次の点を確認してください。

- `copilot-setup-steps` という 1 つの job があります。Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うため、リポジトリの **Actions** タブから手動で実行できます。Copilot が動かすのを待たずに workflow をテストできます。

## ドキュメントを追加する

多くのプロジェクトには、古くなったドキュメントや不足したドキュメントがあります。これは生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成にも役立ちます。まずは、将来レビューできるよう cloud agent に作業を依頼します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択します。

   ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot への割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` などのタイトルの新しい PR を開きます。一覧にない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した todo list を確認します。

> [!NOTE]
> todo list が PR に表示されるまで数分かかることがあります。Copilot は環境を作成し、上記の workflow を実行して、プロジェクトを分析し、問題への取り組み方を決めています。

13. todo list と、実行するタスクを確認します。
14. PR の timeline を下にスクロールし、Copilot が issue の作業を開始したことを確認します。
15. **View session** を選択します。

   ![Copilot session view](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新するために、ブラウザーを再読み込みする必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot がコードを調査して計画を決め、コードを作成する様子を確認します。

完了まで数分かかる場合があります。cloud agent の目的の 1 つは、非同期に作業して開発者が別のタスクを進められるようにすることです。別のタスクを Copilot に依頼してから、アプリケーションの機能を実装する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

Cloud agent の利点は、作業を分けられることです。デザイン チーム向けに関連ゲームを追加する作業を Copilot に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue**、次に **Blank issue** を選びます。
4. **Title** に `Show related games on the game details page` と入力します。
5. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. **Create** を選択します。
7. 右側の **Assign to Copilot** を選択します。

   ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

8. **Assign** を選択します。

issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![issue で Copilot の作業中を示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 件目の依頼を進めています。通常は作業中に監視する必要はなく、完了したらレビューします。次は custom agent を確認して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を割り当てて非同期に作業を依頼できます。技術的負債への対応、新機能の作成、framework の移行などに使えます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- 範囲を明確にした issue の重要性を確認しました。
- issue を Copilot に割り当てました。

次は、custom agent を作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) server も使用できます][cloud-agent-mcp]。前の演習で確認した custom instruction も利用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
