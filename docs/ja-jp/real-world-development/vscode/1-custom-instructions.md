---
title: "演習 1 - custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成型の人工知能 (AI) を使うときは、コンテキストが重要です。タスクの進め方や、Copilot に知っておいてほしい背景情報を伝えるには、[instruction file][instruction-files] を使います。Visual Studio Code などの統合開発環境 (IDE) では、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file が Copilot に適用される方法を確認します。
- 既存の instruction で絞り込み用 publishers helper を作成します。
- `.github/copilot-instructions.md` に新しい標準を追加します。
- 同じ prompt を再実行し、生成コードが新しい標準に従うことを確認します。
- instruction と絞り込み用の最初のデータ スライスを `main` に commit し、次の演習で使えるようにします。

> [!CAUTION]
> 生成コードが標準から外れる場合があります。Copilot は確率的に動作します。この演習では、instruction の更新後に傾向がどう変わるかを確認します。出力を 1 文字ずつ一致させることが目的ではありません。

## instruction file

### 概要

Custom instruction は、Copilot がコーディング スタイルや要件を理解し、より適切な提案やコードを生成できるようにするコンテキストです。使用するライブラリやコーディング規約、コメントの書き方などを指定できます。リポジトリ全体に適用する instruction と、特定のファイル種別やタスクに適用する instruction を作成できます。

リポジトリには、次の 2 種類の instruction file があります。

- `.github/copilot-instructions.md`: リポジトリへのすべてのリクエストで Copilot に送信される単一の file です。技術スタック、プロジェクトの概要、ベスト プラクティスなど、プロジェクト全体に関する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種別向けの file です。TypeScript や Astro などの言語別ガイドライン、UI component や単体テストの作成方法を記載できます。

### instruction file を管理するためのベスト プラクティス

プロジェクトの構造やコーディング標準など、全体に関するガイダンスは `copilot-instructions.md` にまとめます。`*.instructions.md` は、単体テストや Astro component、データ レイヤーなど、特定の file 種別やタスクに使います。自然な言葉で、明確なガイダンスとコード例を示してください。instruction file の作成に唯一の正解はありません。プロジェクトに合わせて試し、改善します。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、目的に合った instruction file を用意します。このプロジェクトには、[UI 更新用 instruction][ui-instructions] や [Astro 用 instruction][astro-instructions] もあります。Copilot に instruction file の生成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使えます。テンプレートを探す場合は、instruction file、custom agent などを集めた [awesome-copilot][awesome-copilot] を確認してください。

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- データ レイヤーには必ず単体テストを用意します。
- user interface (UI) はダーク モードで、現代的な印象にします。
- コードには TypeScript documentation (TSDoc) doc comment を使ってドキュメントを追加します。
- 各ファイルの冒頭に、ファイルの役割を説明するコメント ブロックを置きます。

### Custom instruction

`.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot が使うプロジェクト全体のコンテキストを記載します。`.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

たとえば、リポジトリ全体の instruction には、構築中のもの、プロジェクトの構造、共通のコーディング標準を記載できます。パス単位の instruction には、TypeScript や Astro、UI component、単体テストなどに特化したガイダンスを記載できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成に使われますが、コード補完や next-edit suggestion には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの instruction と `applyTo` front matter がある `*.instructions.md` の両方を使います。GitHub Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の instruction file も[サポートしています][custom-instructions-support]。

### instruction file を確認する

1. Codespace に戻ります。
2. `.github/copilot-instructions.md` を開き、プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** を確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` が対象ファイルを指定する glob を定義していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript test file が対象です。
5. `.github/instructions/drizzle.instructions.md` の末尾を確認し、他の instruction や既存ファイルへのリンクを確認します。パスは instruction file からの相対パスです。
6. `.github/copilot-instructions.md` の **Code formatting requirements** に、コード内ドキュメントのルールがまだないことを確認します。次の手順で TSDoc doc comment と file comment header のルールを追加します。

> [!NOTE]
> **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。

## 変更用ブランチを作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> mode と model の選択は演習によって異なります。必要な場合は手順で指定します。

まず、現在の instruction の状態で prompt を送信し、生成結果を確認します。

1. Copilot Chat panel を開き、**New Chat** を選択してから、前の演習で開いた editor tab を閉じます。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に示します。
3. Chat view の agents dropdown で **Agent** を選択します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認し、helper とテストなどのコードを変更する様子を確認します。
6. `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約により、helper が `db` client を最初の引数に取り、型付き publishers 配列を返すことを確認します。
7. 生成コードに TSDoc doc comment とファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、instruction がなくても doc comment を追加する場合があります。その場合も、更新後の一貫性を確認します。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` に、新しく生成される TypeScript にドキュメントを追加する標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション (27 行目付近) を探し、コード内ドキュメントのルールがまだないことを確認します。
3. 既存の標準の下に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

## prompt を再実行して変化を確認する

1. Copilot Chat で **New Chat** を選択します。
2. `src/lib/publishers.ts` を開き、Copilot の対象ファイルにします。
3. 先ほどと**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc が追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

この helper は次の演習で使います。

## instruction を commit してブランチを push する

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
2. `git status` を実行し、instruction と helper の変更を確認します。
3. instruction と Copilot が helper 用に変更したファイルを stage して commit します。`git status` に追加のファイル (テストなど) が表示される場合は、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチを push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

instruction 更新をバックログの issue と関連付けてプルリクエスト (PR) を作成し、Copilot にマージを依頼します。

> [!NOTE]
> これは Copilot に GitHub 上の操作を依頼する最初の演習です。プロジェクト template では GitHub tool がすでに設定されています。初回利用時に VS Code から GitHub へのサインインを求められた場合は、画面の案内に従ってください。Model Context Protocol (MCP) による接続方法は、後の演習で確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat view を開き、agents dropdown で **Agent** が選択されていることを確認します。
3. 次の prompt を送り、instruction file 更新に関係する issue を探し、現在のブランチから PR を作成するよう依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探し PR を作成する様子を確認します。
5. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次の prompt でマージしてローカルの `main` に戻すよう依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作や shell command の実行を許可する場合は、内容を確認して **Allow** を選択します。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する方法を確認し、VS Code で Copilot Chat を使って次の作業を行いました。

- リポジトリの custom instruction とパス単位の instruction file が Copilot に適用される方法を確認しました。
- 現在の instruction を使って絞り込み用 publishers helper を作成しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じ prompt を再実行し、生成コードが新しい標準に従うことを確認しました。
- instruction と絞り込み用 helper の基盤を `main` に commit し、次の演習で使えるようにしました。

次は、[agent mode][next-lesson]で instruction を使い、コードベース全体に機能を追加します。

## リソース

- [GitHub Copilot の instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction のヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [awesome-copilot: instruction file などのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
