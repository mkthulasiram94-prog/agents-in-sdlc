---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単な更新でも、複数のファイル変更やテスト実行が必要になることがあります。開発者は関連ファイルを探し、変更し、テストを実行し、失敗した場合は原因を調べて、必要な修正を繰り返します。Copilot Agent Mode は統合開発環境 (IDE) 内でより自律的に動作し、プロジェクト構造を調査して必要な変更を行い、テストなどのタスクを実行し、見つかった問題を修正します。この演習では、Agent Mode でサイトに機能を追加します。

> [!NOTE]
> agent mode と cloud agent は異なる体験です。agent mode は IDE 内で対話しながら作業し、cloud agent は issue とプルリクエストを通じて非同期にチームの一員のように作業します。

この演習では、次のことを行います。

- Copilot Agent Mode にプロジェクトを調査させ、関連ファイルを特定して調整された変更を行います。
- user interface (UI) とデータ レイヤーをまたいで機能を実装します。
- 生成された変更とテストをレビューしてから、コードベースにマージします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習で publishers helper を追加しました。残りのデータ レイヤー、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、サイトの現在の動作を確認します。Tailspin Toys は、ボードゲーム向けクラウドファンディング サイトです。単一の Astro app が build 時に静的 Hypertext Markup Language (HTML) を生成します。ページは frontmatter からオブジェクト リレーショナル マッピング (ORM) の Drizzle 経由でローカル SQLite database を直接参照します。別の backend application programming interface (API) や client-side UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` にあり、対話処理には Document Object Model (DOM) API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

1. 前の演習後の `main` に戻っている Codespace を開きます。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. 絞り込み用ブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. サイトを起動します。

   ```bash
   npm run dev
   ```

   サーバーの準備ができると、次のような案内が表示されます。

   ```text
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. `http://localhost:4321` を開くよう促す dialog が表示された場合は、**x** を選択して閉じます。
6. ターミナルに表示された `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択してサイトを開きます。

Codespace の場合、localhost URL は `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。

### Web サイトを確認する

サイトを開いたら、次の主な機能を確認します。

- **Home Page**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択します。mode と model は演習ごとに異なり、必要な場合は手順で指定します。

template からプロジェクトを作成したときに、issue のバックログも用意されています。まず Copilot に内容を表示して優先順位を付けてもらいます。

1. Chat view の agents dropdown で **Agent** を選択します。
2. 利用可能な model の一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> この workshop は、特定の model を推奨するものではありません。手順の作成時には Claude Sonnet 4.5 を使い、体験を一定にするために記載しています。大規模言語モデル (LLM) は確率的に動作するため、生成結果が手順の例と異なる場合があります。これは想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる Model Context Protocol (MCP) command を使うことがありますが、タスクは完了できるはずです。

3. Copilot に次の prompt を送信します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. issue 一覧を取得するため **Continue** を選択します。
5. issue の一覧と優先順位を確認します。

## instruction file を確認する

コード生成を始める前に、Web サイトの機能を追加する方法を説明する [user interface (UI) instruction][ui-instructions] を確認し、Copilot にコンテキストを与えます。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. architecture、component 設計、testability、accessibility のガイダンスを確認します。
3. Astro、Tailwind の Cascading Style Sheets (CSS)、Drizzle などの instruction file へのリンクを確認します。

## 絞り込み機能を実装する

絞り込みを完成するには、データ レイヤー、テスト、ゲーム一覧ページの UI を更新し、すべてのテストを通す必要があります。

1. 現在の Copilot 会話を続けるか、**New Chat** で新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI instruction file を選択する画面](../../../_images/ex3-select-instructions-file.png)

3. agents dropdown で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. model が **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 issue に基づき、次の prompt で実装を依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot が関連するデータ helper、UI、test file を見つけて更新し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトの file を調査する画面](../../../_images/ex3-agent-mode-explores.png)

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の許可を求める Copilot Chat の dialog](../../../_images/ex3-agent-mode-run-tests.png)

8. 結果をレビューします。必要に応じて Copilot はコード生成とテストを繰り返します。

   ![変更提案が完了した Copilot Agent Mode の chat session](../../../_images/ex3-agent-mode-proposed-changes.png)

   > [!NOTE]
   > 作業中に **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。Copilot が完了してから、提案された変更を確認します。

9. 生成コードを確認します。Copilot または他の人工知能 (AI) tool が作成したコードは、必ずレビューしてください。
10. 起動中のサイトに戻り、新しい機能を試します。
11. 動作確認とコードレビューが完了したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加し、生成コードとテストを確認しました。次は、[Playwright MCP server で機能をテストし][next-lesson]、プルリクエストを作成します。

この演習では、次のことを行いました。

- Copilot Agent Mode にプロジェクトを調査させ、関連ファイルを特定して調整された変更を行いました。
- UI とデータ レイヤーをまたいで新しい機能を実装しました。
- 生成された変更とテストをレビューしました。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えた場合に備え、Copilot にページネーションを実装してもらいます。

1. 既存のベスト プラクティスと instruction file を適用して、ページネーションを実装する prompt を作成します。
2. ページ サイズを利用者が選べるようにするか、固定にするかを決め、希望する実装に必要なガイダンスを prompt に含めます。
3. 希望する結果になるまで、Copilot に変更を依頼して反復します。

## リソース

- [Copilot の ask、edit、agent mode の使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
