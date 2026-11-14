---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright の Model Context Protocol (MCP) server でブラウザー上の絞り込み動作を確認しました。このレッスンでは、これらの機能を明確な quality assurance (QA) プロセスを持つ custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する方法を確認します。
- 再利用可能な quality assurance profile を作成して確認します。
- QA agent を選択し、絞り込み issue に対する結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー動作を一貫して確認します。custom agent を使うと、この QA プロセスをまとめて再利用可能なレポートを作成できます。

## custom agent とは

Custom agent は、Markdown profile で定義する、専門分野に特化した Copilot です。profile には agent の目的、instruction、利用可能な tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

各カスタマイズの役割は異なります。リポジトリの instruction はチーム標準を示し、`quality-checks` skill は繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー tool を提供します。QA profile は、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。これらの機能に置き換わるものではなく、新しい会話も必要ありません。

## QA profile を作成する

機能 PR を開く前に、チェック内容と守るべき境界を定義する profile を作成します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次の profile 作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

新しい agent を使う前に、意図した QA の流れと制約が記述されていることを確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は省略できますが、表示名として設定できます。
3. instruction を読み、QA が要件から始め、リポジトリ instruction に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. 根拠を示して報告すること、実装コードを変更する前に確認を求めること、commit や PR 作成を行わないことを確認します。
5. 不足や境界の誤りがあれば、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。profile を作成した後、同じ絞り込み会話を再開して QA を選ぶと、すでにある issue と計画の内容を利用できます。

1. 同じ絞り込み会話を再開します。
2. 次のプロンプトを入力して agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後の agent が利用可能な一覧に表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

3. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

4. QA agent の作業が終わったら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的な役割を追加し、その結果を確認しました。

- custom agent が instruction、skill、MCP tool と連携する方法を確認しました。
- 再利用可能な QA profile を作成して確認しました。
- QA agent を選択し、絞り込み issue に対する結果をレビューしました。

次は、[変更を機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
