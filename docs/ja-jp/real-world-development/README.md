---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントによる作業管理を繰り返しながら進めます。

## シナリオ

架空のボードゲーム向けクラウドファンディング企業 Tailspin Toys に、新しい開発者として参加します。チームのバックログは GitHub issue として用意されています。カテゴリやパブリッシャーによる絞り込みなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、開発者が使う環境で利用できます。作業方法に合った環境を選んでください。各ワークショップでは、それぞれの環境をセットアップし、共通の Tailspin Toys シナリオに取り組みます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進捗を確認し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、custom agent を利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
