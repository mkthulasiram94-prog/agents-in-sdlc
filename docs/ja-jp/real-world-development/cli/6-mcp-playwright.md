---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を設定し、ブラウザーで絞り込み機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、Copilot で追加の自動化を利用したりする必要があります。Model Context Protocol (MCP) server を使うと、Copilot CLI に組み込まれた機能を外部ツールやサービスで拡張できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認します。
- 未設定の場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

単体テストや end-to-end テストは重要ですが、user interface (UI) の更新を検証するには、実際に UI を操作することも必要です。Copilot が利用者のようにサイトを操作できるようにし、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) agent が外部ツールやサービスと通信するための仕組みです。MCP を通じて、エージェントはリアルタイムに外部ツールやサービスと通信し、最新の情報にアクセスして、開発者に代わって操作できます。

MCP server は、エージェントと外部ツールやサービスをつなぎます。各 MCP server は、エージェントが利用できるツールとリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する application programming interface (API) にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化を提供します。

ほかにも多くの MCP server が公開されています。GitHub は、見つけやすさとエコシステムへの貢献を促進する [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server も他の依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP server を追加する

Copilot CLI に Playwright MCP server がまだ設定されていない場合は、サイトを操作できるよう追加します。

> [!NOTE]
> すでに Copilot CLI で Playwright MCP が設定されている場合は、ここでのセットアップを省略して次に進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して保存します。
7. <kbd>Esc</kbd> を押して MCP 画面を閉じます。

## Playwright で機能を確認する

これまでは機能を手動で確認しました。今度は Copilot から Playwright MCP server を使って同じ確認を行います。

1. 次のプロンプトを送信し、Playwright MCP server で動作を確認するよう依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP server を使います。使用する server を明示すると判断を助け、結果の一貫性を高め、token も少し節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はアプリを起動し、Playwright でサイトを操作して、サーバーを停止した後に結果を報告します。

## まとめと次のステップ

Playwright MCP server を使って、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- 未設定の場合は Playwright MCP server を追加しました。
- agent にブラウザーを操作させ、絞り込み機能を確認しました。

次は、skill とブラウザー tool を組み合わせる [quality assurance (QA) custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
