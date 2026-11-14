---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 件のプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連の作業で GitHub Copilot CLI を使いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと事前に用意された絞り込み issue を確認しました。
- 星評価を追加し、転送されたブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- custom instruction でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、ブラウザーで絞り込みを確認しました。
- quality assurance (QA) custom agent を作成して選択し、要件、テスト範囲、skill の結果、ブラウザー上の根拠を確認しました。
- 絞り込み機能全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、任意の cloud delegation に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップでは、更新済み `main` からそれぞれブランチを作り、2 件の PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **絞り込みと品質確認:** 絞り込みを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA profile と関連テストを追加します。

絞り込みの計画から PR 作成までは、同じ会話とブランチを使いました。作業の流れを簡潔にするため、1 件の PR にまとめました。

## 検証の種類

自動テスト、自分でのブラウザー確認、MCP 経由の Copilot によるブラウザー操作でコードを確認しました。`quality-checks` skill は単体テスト、lint、型チェックを実行して、新しい形式で報告しました。QA は PR の前に結果をまとめ、要件とテスト範囲も確認しました。

追加したテストで実際の不足を埋めるようにします。QA の実行で追加テストが不要と判断されることもあります。マージを許可する前に、コードと根拠を確認し、変更後は必要に応じて確認結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストと tool が作業に影響します。このワークショップでは、instruction の更新、skill のカスタマイズ、QA profile の作成、MCP server の設定を行いました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。instruction は標準を定め、skill は繰り返しの作業を記述し、custom agent は専門の役割を定義し、MCP server は外部 tool に接続します。エージェントの要約だけでなく、実際の変更と tool の結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前の方針決定に、**Interactive** は変更を確認しながら進める作業に、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストは引き続き重要です。何を、なぜ、どのように構築するのかを明確に説明すると、結果が大きく変わります。

## さらに試す

基本のワークフローに加えて、次の CLI 機能も試せます。

- `/review` で code review agent に変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して進めます。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールを上達させるには、使い続けることが一番です。製品コード、趣味のコード、作りたいと思いながら手をつけていない小さなアプリで試してください。チームで学びを共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。別の作業環境を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
