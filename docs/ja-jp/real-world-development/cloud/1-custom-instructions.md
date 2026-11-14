---
title: "演習 1 - custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成型の人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、コンテキストを参照できるようにします。[instruction file][instruction-files] では、必要な作業だけでなく、その進め方も指定できます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の harness とは異なり、ここでは更新前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで影響を比較するのが難しいためです。この harness の後半で、instruction file が cloud agent のプルリクエストにどう反映されたかを確認します。

## instruction file

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- データ レイヤーには必ず単体テストを用意します。
- user interface (UI) はダーク モードで、現代的な印象にします。
- コードには TypeScript documentation (TSDoc) doc comment を使ってドキュメントを追加します。
- 各ファイルの冒頭に、ファイルの役割を説明するコメント ブロックを置きます。

instruction file を使い、これらのプラクティスに沿って Copilot が作業できるようにします。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件などのコンテキストを Copilot に渡せます。リポジトリ全体にも、特定のファイル種別やタスクにも instruction を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。技術スタック、プロジェクト概要、ベスト プラクティスなど、全般に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けのガイドラインを記載します。TypeScript や Astro などの言語、UI component や単体テストなどのタスクに使えます。

> [!NOTE]
> ほかの instruction 形式への対応状況は環境によって異なります。特定の形式を使う前に、[custom instruction support reference][custom-instructions-support] を確認してください。

### instruction file を管理するためのベスト プラクティス

スターター プロジェクトの例は、一般的な管理方法を示しています。

- `copilot-instructions.md` には、プロジェクトの説明、構造、全体的なコーディング標準などを記載します。
- `*.instructions.md` は、テスト、Astro component、データ レイヤーなどのファイル種別や、特定のタスク向けに使います。
- 自然言語で明確に記述し、望ましいコードと望ましくないコードの例を示します。

instruction file の作成方法や AI の使い方に唯一の正解はありません。プロジェクトに合う方法を試してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、十分な instruction file を用意します。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] などのガイドがあります。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各 surface で instruction file を生成する方法もあります。template や出発点を探す場合は、instruction file や custom agent などが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## このプロジェクトの custom instruction file を確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` が含まれています。エディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。repository instruction は、このリポジトリで Copilot とやりとりするたびに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` が、instruction の適用対象を決める glob を定義していることを確認します。
5. このプロジェクトの単体テストに関する instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾で、他の instruction file とプロジェクト内の既存ファイルへのリンクを確認します。大きな instruction を再利用しやすい単位に分け、Copilot が参照できる例を示せます。

> [!NOTE]
> `.github/copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。

## リポジトリ標準を追加する

ここでは、プロジェクト全体に適用する標準として、TSDoc doc comment とファイル レベルのコメント ヘッダーを追加します。次の演習で issue を割り当てるとき、cloud agent はこの標準を使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. **Code formatting requirements** セクション (27 行目付近) を探し、コード内ドキュメントのルールがないことを確認します。
4. 既存標準の下に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、全体的なガイダンスには `.github/copilot-instructions.md`、特定の言語、ファイル種別、タスクには `*.instructions.md` を使います。ここで追加した doc comment のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## instruction の更新を commit、push、マージする

Cloud agent は issue の対象ブランチから instruction file を読み込みます。次の演習では `main` からブランチを作成するため、cloud agent が更新内容を使えるように、変更を `main` にマージします。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で cloud agent が更新済み instruction を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作る手順を省略して、変更を `main` に commit します。

## まとめと次のステップ

この演習では、次のことを行いました。

- リポジトリの custom instruction とパス単位の instruction file が Copilot に適用される方法を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。

後の演習で、cloud agent が作成する PR にこの標準がどう反映されるかを確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良い custom instruction を作成する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
