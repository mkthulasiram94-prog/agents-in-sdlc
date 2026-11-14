---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー体験の改善と機能追加を目的とする 3 件のタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、進捗を監視することも重要です。

タスクを管理する方法の 1 つに、GitHub.com の [agents page][agents-page] があります。この mission control では、未クローズのプルリクエスト (PR) があるすべての agent task を確認し、実行内容を調べ、進行中のセッションに追加のガイダンスを送れます。

このレッスンでは、次のことを行います。

- agents page で cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

高コントラスト モードの作成を依頼した後、チームはライト モードも追加したいと考えました。すでにサイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが適切です。agent にライト モードも追加するよう案内します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents page](https://github.com/copilot/agents) を開きます。
2. メイン領域と左側の一覧に、Copilot に割り当てたタスクが表示されていることを確認します。たとえば、コード ドキュメントの更新、ゲーム変更 API の生成、Web サイトへの高コントラスト モードの追加などです。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、環境の作成、Model Context Protocol (MCP) server の設定、タスクを完了するための作業などが表示されます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインで、PR と変更されたファイルを確認します。

## cloud agent に追加の指示を送る

進行中の agent に、高コントラスト モードに加えてライト モードも作成するよう依頼します。

1. 高コントラスト モードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なる場合があります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり作業が始まるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次の prompt を入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents page のセッション案内用ダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が依頼を認識し、作業に追加したことを確認します。

## Copilot に作業を続けてもらう

Copilot は、メッセージ送信時に進めていた手順が終わった後、追加依頼を作業に取り込みます。完了するまで数分かかることがあります。その間に、これまでに学んだ内容を振り返ってください。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- agents page と mission control を使って cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot agents page では、進行中と完了済みのタスクを追跡し、作業内容を確認して、進行中のタスクに追加のガイダンスを送れます。次は、Copilot が作成した [プルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [custom agent について][custom-agents]

---

| [← 前のレッスン: custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
