---
title: "演習 4 - custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発フローの特定のタスクや分野に特化した人工知能 (AI) assistant を作成できます。`.github/agents` に Markdown file を置き、focused instruction、ベスト プラクティス、コーディング パターン、分野固有の知識を指定します。チームの専門知識を再利用可能な agent にまとめられます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディングに従う security agent、一貫した test pattern を保つ testing agent などです。

Custom agent はプロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には `name` と `description` を含む YAML Ain't Markup Language (YAML) frontmatter と、agent の動作や専門知識を定める Markdown prompt が含まれます。

### custom agent と agent skill の違い

[Agent skill][agent-skills-concept] と custom agent はどちらも、AI に作業方法を伝える Markdown file で定義できます。わかりやすく言うと、**custom agent は作業者、skill は tool** です。custom agent は専用のコンテキスト ウィンドウを持ち、skill や他の agent を調整できます。この演習では、アクセシビリティ agent がガイドラインに沿ってサイトを確認し、pull request workflow や test 実行用の skill を使う場合があります。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。環境やシナリオに合わせて試して改善します。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダーク テーマは文字と背景のコントラストが不十分で読みづらいという意見を受け、デザイン チームは利用者が切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、この演習では custom agent を使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat のアクセシビリティ agent を使って、高コントラスト モードを実装します。

## アクセシビリティ custom agent を確認する

まず、アクセシビリティ custom agent を確認し、Exercise 3 の絞り込み用プルリクエスト (PR) とは分けて `main` から作業ブランチを作成します。

1. Codespace に戻り、ターミナルを開いて次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```
2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> custom agent には `name` と `description` を含む frontmatter が必要です。

4. アクセシブルな Web サイト向けの責務、ベスト プラクティス、Hypertext Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例、よくある問題を確認します。

## Copilot Chat で custom agent を使う

VS Code の Copilot Chat view 下部にある agents dropdown では、`.github/agents` に定義した custom agent を選択できます。

> [!TIP]
> **Copilot Chat を開く**
>
> Copilot Chat では演習ごとに mode と model を選択します。必要な場合は手順で指定します。

1. Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択します。
2. Chat view の agents dropdown で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

3. chat view 下部の agents dropdown (既定では **default** が表示されます) を開きます。
4. 利用可能な agent の一覧から **Accessibility agent** を選びます。
5. 次の prompt を送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

6. Copilot Chat がコードベースを調査して変更を提案し、ファイルを編集する様子を確認します。
7. **Files changed** の表示で、このセッション中に変更されたファイルを確認します。

> [!NOTE]
> Astro component、CSS、関連 test などのファイルが実際に変更されるため、数分かかることがあります。次の演習では、作業中の内容を確認して案内します。

## まとめと次のステップ

この演習では、次のことを行いました。

- 既存のアクセシビリティ custom agent を確認しました。
- Copilot Chat でアクセシビリティ agent を使い、高コントラスト モードを実装しました。

次は、[agent の作業を監視して案内し][next-lesson]、同じセッションにライト モードを追加します。

## リソース

- [custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
