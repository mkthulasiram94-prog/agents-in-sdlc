---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、使い方と用意された絞り込み issue を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部 tool と連携できます。このレッスンでは、インストールして認証し、ワークショップ用リポジトリを信頼して簡単な会話を試します。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- workshop リポジトリを信頼して簡単な会話を試します。
- 組み込みの GitHub Model Context Protocol (MCP) server で絞り込み issue を探します。

## シナリオ

チームは、増え続けるバックログに対応するため AI agent を使い始めています。Copilot CLI は、多くの開発者が普段作業するターミナルで利用できます。このレッスンでは、インストールと認証を完了し、以降のワークショップに備えます。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証が求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は device code の手順に従い、認証して Copilot CLI を許可します。
3. Copilot CLI に次の prompt が表示されることを確認します。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されていることがあります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本を確認する

通常の shell prompt でのコマンドは Codespace で実行されます。Copilot CLI の起動後は、自然言語を agent に送り、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問で Copilot が動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込み GitHub MCP server が利用可能であることを確認します。
6. Copilot に絞り込み issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue を読みます。最初の小さな変更の後で使用します。

> [!TIP]
> 通常の Copilot CLI session は、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree を作成しません。変更ごとに feature branch を作成します。

## workshop 用の shortcut を使う

Copilot CLI は通常、許可された範囲外の tool を使う前に確認を求めます。このワークショップでは、Codespace 内の承認 prompt を省略できる `--yolo` を使います。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や仮想マシン (VM) のような分離された環境でのみ使い、日常の開発で既定値として alias しないでください。詳しくは [Allowing and denying tool use][allow-all-warning] を参照してください。

`--enable-all-github-mcp-tools` は、後のレッスンで issue や PR を扱うための GitHub MCP 読み取り/書き込み tool を有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証済み GitHub resource は実際のものです。公開やマージの前に変更をレビューします。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリ root から再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 簡単な質問をもう一度送り、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で instruction の更新や agent の追加後に、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- workshop repository を信頼し、簡単な会話を試しました。
- 組み込み GitHub MCP server で絞り込み issue を見つけました。

次は、[最初の変更に取り組み][next-lesson]、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
