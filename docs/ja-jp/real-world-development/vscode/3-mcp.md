---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: custom agent →][next-lesson]

Agent Mode で絞り込み機能を構築しました。プルリクエスト (PR) を作成する前に、ブラウザーで動作を確認します。Playwright Model Context Protocol (MCP) server を接続し、Copilot に実際のブラウザーを操作させて機能をテストし、その後ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP server が Copilot を拡張する方法を学びます。
- Playwright MCP server を workspace に追加します。
- Copilot にブラウザーから絞り込み機能をテストさせます。
- ブランチを公開して絞り込み用プルリクエスト (PR) を作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外にある tool と連携すると、さらに強力になります。Model Context Protocol (MCP) は、Copilot agent が外部 tool やサービスと通信するための標準です。

![agent mode がコンテキスト、大規模言語モデル (LLM)、MCP server や VS Code 拡張機能が提供する tool とやりとりする仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、人工知能 (AI) agent は外部 tool や service とリアルタイムに通信できます。resources を使って最新情報を取得し、tools を使って開発者に代わる操作を実行します。MCP server は、AI agent と既存の application programming interface (API) やローカル tool などの外部機能との通信を管理し、agent が利用できる resources と tools を提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository を管理する API にアクセスします。repository の作成や更新、issue や pull request の管理などを agent が実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作します。Web ページへの移動、フォームへの入力、button のクリックなどを agent が実行できます。

ほかにも多くの MCP server があり、GitHub は見つけやすさを高める [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP server も他の依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証し、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密リソースや操作へのアクセス許可に注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` は、VS Code workspace で利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` server が設定済みであることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` entry はプロジェクト template に含まれています。remote GitHub MCP server を使い、Copilot が前の演習でバックログ issue を読めた理由です。ローカルへのインストールは不要です。VS Code は HTTP で接続し、初回利用時に GitHub で認証します。

## Playwright MCP server を追加する

1. `.vscode/mcp.json` で `github` の隣に `playwright` entry を追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

`github` server と異なり、`playwright` はローカル server です。VS Code が `npx @playwright/mcp@latest` を実行して起動します。Codespace には表示用デスクトップがないため、`--headless` でブラウザーを画面表示せずに実行します。

> [!NOTE]
> Tailspin Toys は end-to-end test に Playwright を使うため、必要なブラウザーは通常インストール済みです。ブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行します。

## server を起動して信頼する

VS Code は Copilot が tool を初めて必要としたときに MCP server を起動し、信頼を確認します。

1. `.vscode/mcp.json` の `playwright` entry 上にある **Start** action を選択し、設定が正しいことを確認します。
2. 信頼を求められたら設定を確認し、server を起動するため信頼する選択肢を選びます。

ここで起動するのは任意です。省略した場合は、次の手順で Copilot が tool を必要とした時点で server が起動し、信頼の確認が表示されます。

## 絞り込み機能をテストする

Playwright MCP server を使うと、agent がページを開き、移動し、filter を設定して結果を読み取れます。server を接続してアプリを起動した状態で、次の prompt を Copilot に送信します。

1. Copilot Chat に次の prompt を送信します。

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

Copilot は Playwright MCP server からブラウザーを起動し、各手順を確認して結果を報告します。issue の受け入れ条件に照らして報告を読み、問題があれば質問するか、PR を作成する前にコードの修正を依頼します。

Playwright MCP server はスクリーンショットではなく、ページの [accessibility tree][playwright-mcp-server] を使います。agent は button、link、list item など、ラベル付きの構造化要素を調べます。これは支援技術と同様の情報に基づくため、機能確認とあわせて簡単なアクセシビリティ確認にもなります。

> [!NOTE]
> テストするにはアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、prompt を送る前に起動します。初回利用時にブラウザーが必要だと報告された場合は、`npx playwright install chromium` を実行して再試行します。

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** panel を開き、Copilot の変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。
4. **Publish** を選択してブランチをリポジトリに push します。

   ![Copilot の変更を示す Source Control panel](../../../_images/ex3-source-control-changes.png)

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) などで PR を作成できます。ここでは Copilot に関連 issue を検索させ、PR と関連付けます。

1. Copilot Chat panel で **New Chat** を選択します。
2. agents dropdown で **Agent** を選択します。
3. 次の prompt を送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、Copilot に情報収集や操作を許可します。GitHub tool の初回利用時は、認証を求められることがあります。
5. Copilot が issue を検索して PR を作成することを確認します。
6. Copilot が表示したリンクから PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP server が Copilot に tool を提供する仕組みを学びました。
- Playwright MCP server を workspace に追加しました。
- Copilot にブラウザーで絞り込み機能を手動テストさせました。
- ブランチを公開して絞り込み用 PR を作成しました。

次は、[アクセシビリティのレビューなどに使う custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
