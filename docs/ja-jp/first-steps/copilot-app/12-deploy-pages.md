---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app を使って Pages workflow を設定し、プルリクエストをレビューして Space Quiz の公開を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages にデプロイして、Space Quiz を公開します。Copilot は workflow の準備とデプロイの案内を行いますが、変更をレビューし、サイトが公開されたことを確認するのは開発者です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages デプロイの設定を依頼します。
- マージ前に workflow とプルリクエストをレビューします。
- デプロイの進行を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、利用者が簡単に試せる場所が必要です。単一ファイルのサイトを GitHub Pages で公開すると、サーバーや実行時依存関係を追加せず、公開 URL から利用できるようになります。

## Copilot にデプロイの準備を依頼する

まず、Copilot にレビュー用の変更案を準備してもらい、公開操作はレビュー後に許可します。

> [!WARNING]
> プルリクエストを作成すると、コードの commit と push が行われる場合があります。プロンプトを送る前に、承認設定と送信先リポジトリを確認してください。**Approve all** が有効な場合、別途確認を求められないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## workflow とプルリクエストをレビューする

マージを許可する前に、README と workflow の変更を確認し、既定のブランチからデプロイされることと必要な設定を検証します。

1. README は、リポジトリにまだない場合に限って追加されていることを確認します。
2. workflow のビルドとデプロイの手順、トリガー、権限を含め、プルリクエストの変更をレビューします。
3. workflow が、マージされていないプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新後の差分をレビューします。マージを許可する前に、必須のプルリクエスト チェックが成功するまで待ちます。

## マージとデプロイを許可する

レビュー済みの変更と必要な設定を確認した後にのみ、エージェントにマージと公開を許可します。

> [!WARNING]
> 次のプロンプトを送ると、レビュー済みのプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作について別途確認が求められない場合があります。

`<PR_NUMBER>` をレビューしたプルリクエストの番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![マージ済みの Space Quiz pull request、Build website、Upload artifact、Deploy to Pages の各 step が成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

Actions の実行結果だけでなく、公開 URL でサイトが読み込まれることも確認します。

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで完了とみなさないでください。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Copilot に GitHub Pages デプロイの設定を依頼しました。
- マージ前に workflow とプルリクエストをレビューしました。
- デプロイの進行を確認し、公開されたサイトを検証しました。

Space Quiz が公開されていることを、エージェントの報告だけに頼らず確認しました。次は、[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
