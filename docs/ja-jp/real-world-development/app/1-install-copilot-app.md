---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、template から作成したリポジトリを接続し、workspace と簡単なチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使用するデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、継続的インテグレーション (CI) pipeline をすぐに利用できます。自分ですべての作業を行う代わりに、分離された workspace で複数の agent を並行して動かし、反復作業を自動化する workflow 向けに設計されています。Node.js のインストールとプロジェクトの準備ができたので、app をインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトを app に追加します。
- template が用意したバックログを含め、workspace を確認します。
- app 自体について知るため、簡単なチャットを試します。

## シナリオ

チームは、増え続けるバックログに取り組むため人工知能 (AI) agent を導入しています。Copilot app では、issue の選択、agent の実行、変更のレビュー、プルリクエスト (PR) のマージを一か所から指示できます。このレッスンでは、app をインストールして接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** policy を有効にする必要があります。

## GitHub Copilot app をインストールして設定する

GitHub Copilot app は Windows、macOS、Linux で利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用している platform 用の app をダウンロードし、案内に従ってインストールします。
3. インストール後に app を開きます。
4. **Sign in to GitHub** を選択し、GitHub.com または GHE.com アカウントでサインインします。データ所在地を使う GitHub Enterprise Cloud の場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` の host name を入力します。

   GitHub Enterprise Server を使用する場合は、まず cloud account で onboarding を完了します。その後 **Settings**、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインイン ガイド][ghes-sign-in]に従って、ブラウザーで app を承認します。

5. onboarding で repository を選ぶ場合は、template から作成した Tailspin Toys repository を選択します。repository の選択を省略して、onboarding 後に追加することもできます。
6. theme を選んで onboarding を完了します。
7. sidebar の **Sessions** に repository が表示されることを確認します。すでに表示されていれば、再度追加する必要はありません。
8. repository がまだ接続されていない場合は、**Sessions** の横にある **+** を選択して session を作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた owner と repository name を使います。
9. repository が sidebar に表示されることを確認します。この画面では `tailspin-toys` などの repository folder が **Sessions** の下にまとまり、その下に個々の session が表示されます。

## workspace を確認する

プロジェクトを接続したら、sidebar の各領域を確認します。

- **New**: Copilot との新しい chat session を開始します。
- **Pull requests**: app に統合された GitHub の pull request (PR) を参照、絞り込み、CI 状態の確認、レビューに使います。
- **Issues**: GitHub issue を参照、絞り込み、issue からの session 開始に使います。
- **Automations**: schedule またはオンデマンドで実行する agent task を保存します。やることリストや定期的な保守などを任せられます。まとめでは次の手順として紹介しますが、別の演習ではありません。
- **Customize**: Model Context Protocol (MCP) server、plugin、skill などの component を追加します。後で Playwright MCP を設定します。
- **Chats**: branch や専用 workspace が不要な質問やブレインストーミング向けの軽い会話です。最初の chat を始めると表示されます。表示されない場合は **Sessions** の横にある **+** を選択して、最初の chat を開始します。
- **Sessions**: repository folder と agent session をまとめます。repository 名 (例: `tailspin-toys`) を選択すると repository view が開き、その下の session を選ぶと会話に戻ります。session ごとに分離された workspace が使われるため、複数の作業を並行しても変更が競合しません。星評価を追加する際に最初の session を開始します。

ワークショップを進めながら workspace の使い方を確認します。

> [!TIP]
> 操作がわからない場合や機能の有無を知りたい場合は、Copilot に質問して案内を受けてください。

### 用意されたバックログを見つける

template 作成時に用意されたバックログを確認します。

1. sidebar の **Sessions** で Tailspin Toys repository folder を選びます。個々の session ではありません。repository 上部の navigation で **Issues** を選択します。
2. issue 番号を前提にせず、タイトルで次の issue を探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. issue を選択して詳細を読みます。issue から agent session を開始できます。最初の小さな変更を完了した後、絞り込み issue から作業します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は、Copilot app に追加した repository の項目だけを表示するよう自動的に絞り込まれます。他の repository の作業項目を見るには、その repository を app に追加します。

## クイックチャットを試す

**Quick chat** では、branch や worktree を作らずに質問やブレインストーミングができます。その場限りの質問に適しています。

1. sidebar の **Chats** の横にある **+** を選択します。**Chats** がまだ表示されない場合は、**Sessions** の横にある **+** を選択して最初の chat を始めます。
2. app 自体の session について、次の質問を送ります。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各 session が分離された git worktree を使うため、複数の agent を並行して動かしても変更が競合しないことを確認します。会話は続けることも、新しい chat を開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して workspace を確認しました。具体的には、次のことを行いました。

- app をインストールして GitHub にサインインしました。
- GitHub repository からプロジェクトを追加しました。
- workspace を確認し、**Issues** で用意されたバックログを見つけました。
- quick chat を使って、その場限りの質問をしました。

次は、[最初の agent session を開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app で agent session を操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
