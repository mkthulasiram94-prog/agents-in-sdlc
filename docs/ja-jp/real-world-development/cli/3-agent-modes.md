---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue から絞り込み機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、堅実なプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って、適切なものを適切な方法で構築できるよう設計されています。このレッスンから、issue を起点に機能を生成し、コードの妥当性と動作を確認して、プロジェクトにマージするエージェント駆動の開発プロセスを進めます。

> [!NOTE]
> 機能開発の間は、同じ会話とブランチを使います。通常はファイル種別ごとにブランチや PR を分けますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能開発を続ける中で、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA agent を作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの issue には機能が記されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な点もあります。Plan モードで決定し、範囲を明確にした実装を Autopilot に任せます。

## 背景

人工知能 (AI) コーディング エージェントを開発フローに導入しても、基本は変わりません。一般的な開発フローは次のとおりです。

1. 作業内容を記述した issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの開発フローは上記の流れを基本にしています。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントは先に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: エージェントは自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

絞り込み作業の前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue またはプルリクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てることで、機能の実装やタスクの実行を Copilot に任せる前に、方針を決められます。複雑な作業では、実装前に計画することをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. issue への参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調査し、実装方法を検討して計画を作成します。

3. 絞り込みの動作について質問されたら、希望する方法で回答します。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 決定した内容と合わない部分があれば、修正を依頼します。

## Autopilot を承認する

計画を確認したら、Copilot に実装を任せます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同等のラベルの選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している場合やポートが競合する場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** でないことを確認します。
2. `/diff` を入力し、絞り込みの実装とテストを確認します。
3. issue と計画で決めた内容を基準に、結果を確認します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードが問題なさそうなら、アプリを起動し、Codespaces の転送ポートからサイトを開いて動作を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、パブリッシャー、およびその組み合わせで絞り込みます。
4. リセット時と結果がない場合の動作が、issue と決定内容に合うことを確認します。
5. Codespace に戻り、起動した開発サーバーを Copilot に停止させます。

## まとめと次のステップ

会話モードを使い分けて、機能を構築しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[custom instruction を使って][next-lesson]、コードが文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
