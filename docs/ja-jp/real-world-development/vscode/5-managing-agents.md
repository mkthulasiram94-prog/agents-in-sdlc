---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: custom agent][previous-lesson] |
|:--|

Copilot を agent mode で使うと、コードベースを調査し、変更を提案し、ファイルを編集して、コマンドを実行します。VS Code の Copilot Chat では tool call やファイル編集を確認できます。差分を確認して個別の変更を受け入れるか取り消し、追加 prompt で作業を調整できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイル編集をレビューして受け入れます。
- セッション中にライト モードの追加を依頼します。

## シナリオ

[演習 4][previous-lesson] で使ったアクセシビリティ custom agent は、高コントラスト モードを実装しています。作業中、デザイン チームからライト モードの切り替えも追加してほしいという依頼がありました。新しい会話を始めずに、進行中のセッションで依頼を追加します。

## Copilot の進捗を監視する

Copilot Chat view には、agent が調査するファイル、実行するコマンド、提案する編集が表示されます。編集の差分には **Keep** と **Undo** の操作があります。

1. 演習 4 で使っている Copilot Chat view に戻ります。
2. agent の作業中に chat を確認します。ファイルの読み取り、編集、実行を求められた terminal command が表示されます。
3. ファイル編集の差分で、**Keep** を選ぶと変更を受け入れ、**Undo** を選ぶと変更を取り消します。
4. chat 上部の **Files changed** を開き、このセッションで変更されたファイルを確認します。

> [!NOTE]
> terminal command の実行などに許可が必要な場合、agent は **Allow** を表示して一時停止します。内容を確認してから許可します。

5. agent が高コントラスト モードを実装し、変更をまとめるまで待ちます。

## ライト モードを追加するよう agent に依頼する

同じ会話で追加依頼すると、agent は先ほどの component、スタイル、local storage のパターンなどのコンテキストを保ったまま作業できます。

1. Copilot Chat の入力欄に次の prompt を送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. agent が既存の変更を確認し、ライト モードを追加する様子を見ます。
3. 新しい差分を確認し、**Keep** または **Undo** を選択します。
4. 希望と異なる方向に進んだ場合は、chat 入力欄の **Stop** で停止し、明確な追加 prompt を送信します。

> [!TIP]
> 進行中の session に追加 prompt を送ると、最初からやり直すより早く修正できます。たとえば、Cascading Style Sheets (CSS) の簡素化、既存の設定 panel への統合、両方の toggle 用 keyboard shortcut の追加などを依頼できます。

## 作業内容をレビューする

commit 前に agent が変更したファイルを確認します。

1. VS Code で **Source Control** view を開きます。
2. 変更されたファイルの一覧を確認します。Astro component、style、関連 test などが含まれる場合があります。
3. 変更ファイルをいくつか開き、Accessible Rich Internet Applications (ARIA) attribute、keyboard navigation、semantic HTML、local storage による保存を確認します。
4. 問題がなければ **Source Control** panel から変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

この演習では、次のことを行いました。

- アクセシビリティ agent の作業をリアルタイムで監視しました。
- 提案されたファイル編集をレビューして受け入れました。
- セッション中に追加 prompt を送信し、高コントラスト モードとともにライト モードを追加しました。

次は、[アクセシビリティの変更をプルリクエストとして公開し][next-lesson]、この lab で作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [agent mode を使う][agent-mode]

---

| [← 前のレッスン: custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
