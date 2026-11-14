---
title: "演習 3 - custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発フローの特定のタスクや分野に特化した artificial intelligence (AI) assistant を作成できます。リポジトリの `.github/agents` フォルダーに Markdown file を置き、focused instruction、ベスト プラクティス、コーディング パターン、分野固有の知識を指定します。チームの専門知識を再利用可能な agent にまとめられます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディングの実践に従う security agent、一貫したテスト パターンを守る testing agent などです。

Custom agent は、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各ファイルには、少なくとも `name` と `description` を含む YAML Ain't Markup Language (YAML) frontmatter と、agent の動作、専門知識、instruction を記載した Markdown prompt が含まれます。

### custom agent と agent skill の違い

custom agent と [agent skill][agent-skills-concept] はどちらも、主に Markdown file で定義し、AI に作業方法を伝えます。わかりやすく言うと、**custom agent は作業者、skill は tool** です。

Custom agent は専用のコンテキスト ウィンドウを持ち、skill や別の agent を調整して作業できます。この演習のアクセシビリティ custom agent は、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエスト workflow skill やテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。AI の他の用途と同じように、環境やシナリオに合わせて試して改善します。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、custom agent を使って次のことを確認します。

- custom agent の定義方法
- custom agent へのタスク割り当て

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマは文字と背景のコントラストが不十分で、読みづらいという意見がありました。デザイン チームは、利用者が切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、custom agent を使って機能をすばやく作成します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。内容を確認して、Copilot をどのように導くかを理解します。

Codespace に戻り、次の手順で agent file を確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> custom agent には `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイト向けの責務、ベスト プラクティス、Hypertext Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例、よくある問題を確認します。

## issue を作成して割り当てる

Mission control では、Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを送れます。高コントラスト モードを作成する issue を Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選びます。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。

   ![custom agent と Accessibility が選択された Copilot への割り当て画面](../../../_images/ex5-select-custom-agent.png)

9. **Accessibility agent** を選択します。
10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents] を確認しました。custom agent を使うと、チームの知識や標準を再利用できる agent にまとめられます。

この演習では、次のことを確認しました。

- custom agent の定義方法
- custom agent へのタスク割り当て

次は、mission control から [agent セッションを監視して案内します][next-lesson]。

## リソース

- [custom agent について][custom-agents]
- [組織で custom agent を使う準備][org-custom-agents]
- [Enterprise で custom agent を使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
