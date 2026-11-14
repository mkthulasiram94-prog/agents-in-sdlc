---
title: "レッスン 4 - custom instruction で Copilot を導く"
description: "repository instruction を確認してドキュメント標準を追加し、絞り込みコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成型の人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのガイダンスを参照できるようにします。[instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も指定できます。絞り込み機能を実装したので、このレッスンでは Copilot が使った instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- repository instruction とパス単位の instruction file が agent に適用される方法を確認します。
- コーディング標準が守られるよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- コメントにはコードの言い換えではなく、意図と自明でない判断を記述します。
- `db/` と `src/lib/` の export 関数には、目的、parameter、戻り値を説明する TypeScript documentation (TSDoc)/JavaScript documentation (JSDoc) を付けます。該当する場合は注入可能な `db` 引数も含めます。
- 再利用可能な Astro component は `Props` 契約を文書化し、関連コードが変わったときにコメントも更新します。
- 既存の書式と lint のガイダンスを維持します。

instruction file を使い、これらのプラクティスに沿って Copilot が作業できるようにします。

## instruction file

Custom instruction を使うと、コーディング規約や要件などのコンテキストを Copilot に渡せます。repository 全体にも、特定の file type や task にも instruction を作成できます。

- `.github/copilot-instructions.md` は repository への**すべて**の request で Copilot に送信される単一の instruction file です。project-level の説明を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、file type、task 向けのガイドラインを記述します。

> [!NOTE]
> ほかの instruction 形式への対応状況は環境によって異なります。特定の形式を使う前に、[custom instruction support reference][custom-instructions-support] を確認してください。

## このプロジェクトの custom instruction file を確認する

この starter project に用意された instruction file を、変更前に確認します。

1. Codespace に戻ります。
2. Codespaces editor で `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。repository instruction は、このリポジトリで Copilot とやりとりするたびに適用されます。
4. `.github/instructions` folder を開き、Astro、Drizzle data layer、test などの instruction を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` が、instruction の適用対象を決める glob を定義していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の instruction file と既存のプロジェクト file への link を確認します。instruction を再利用しやすい小さな単位に分け、Copilot が参照できる例を示せます。

> [!NOTE]
> `.github/copilot-instructions.md` にはコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。

## チームのガイダンスに合わせて instruction file を更新する

絞り込み用 helper の生成で確認したように、Copilot は現在の instruction に従います。新しい TypeScript で TSDoc を使うよう、repository-level の標準を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting guidance** セクション (35 行目付近) を探します。
3. 最後の bullet として、次の Markdown を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話開始時に repository instruction を読み込みます。instruction の更新後に絞り込み会話を再開し、機能のコンテキストを保ったまま適用します。

1. 絞り込み会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. repository root の `game-filters-cli` branch から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

3. 求められた場合は絞り込み会話を選び、`game-filters-cli` branch が checkout されていることを確認します。
4. Copilot に次の prompt を送信して、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. `/diff` を入力し、変更された TypeScript file を確認します。新しい TSDoc が追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、次の作業を行いました。

- repository instruction とパス単位の instruction file が agent に適用される方法を確認しました。
- コーディング標準が守られるよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、test と lint を一貫して実行します。

## リソース

- [Copilot CLI の custom instruction を追加する][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良い custom instruction を作成する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
