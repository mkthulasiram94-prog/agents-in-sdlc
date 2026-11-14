---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして絞り込み機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードが動作するか手動で検証したり、instruction file で標準に従わせたりできます。テスト、lint、継続的インテグレーション (CI) の他の作業には **agent skill** が適しています。skill は、こうした作業の実行方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- レポートの形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` skill を使っています。チームは、結果が読みやすくなるようレポートを改善したいと考えています。

## instruction、script、resource

Agent skill は、再利用可能なタスク用の instruction、実行可能な script、補助 resource をまとめ、必要なときに agent が読み込めるようにします。skill の基本は、名前と説明を含む frontmatter を持つ `SKILL.md` と、その概要、呼び出すタイミングなどを示す Markdown instruction です。script や他の resource 用のサブフォルダーを含めることもできます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクト既存のコマンドを使うため `SKILL.md` のみで構成されています。

skill は、チームで共有して再利用する repository asset として `.github/skills` に置くことも、ユーザー用の `~/.copilot/skills` に置くこともできます。

## skill を確認する

Tailspin Toys チームが単体テスト、lint、型チェックに使う `quality-checks` skill を確認します。

1. Codespace に戻り、Codespaces editor で `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出すタイミングの判断に役立ちます。
3. instruction を読み、テストと lint の進め方を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前に skill を実行する

skill は Copilot CLI から直接、または自然言語で実行できます。3 種類のチェックを依頼します。

1. Interactive モードで絞り込み会話に戻ります。
2. 次の prompt を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 完了時のレポートを確認します。

## レポートをカスタマイズする

どの command を実行したか、成功したか、tool が何を報告したかがわかるようにレポートを改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

ファイルは自動的に保存されます。

## 更新した skill を実行する

Copilot CLI は会話を再起動せずに、編集した skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じ prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 結果を確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。`quality-checks` skill を確認し、結果の形式をカスタマイズして、再読み込み後に実行しました。この変更を絞り込み機能の PR に含めます。次は、[Playwright MCP server で Copilot にサイトを操作させます][next-lesson]。

## skill の例

次の community example は参照用であり、追加の作業ではありません。

- [Agent Skills specification][skill-spec]
- [コントリビューション workflow: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
