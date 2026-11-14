---
title: "演習 5 - GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このワークショップでは、コードへのドキュメント追加、関連ゲーム機能、高コントラストとライト モードを含むアクセシビリティ機能を Copilot に依頼しました。コード変更を確認し、必要なら Copilot に修正を依頼します。

### シナリオ

生成型の人工知能 (AI) を導入しても、ソフトウェア設計と開発運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を保つための制約があります。Model Context Protocol (MCP) server を含め、外部リソースへのアクセスは既定で制限されます。

- Copilot はリポジトリに読み取りアクセスでき、書き込みは作業用ブランチに限られます。
- Cloud agent は GitHub Actions 内に一時的な環境を作成して実行されます。
- GitHub Actions workflow の実行には、人による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます][agent-firewall]。MCP server も対象です。

## 生成されたドキュメントをレビューする

最初に、コードにドキュメントを追加する Copilot cloud agent の pull request (PR) を確認します。

> [!NOTE]
> PR に GitHub Copilot が firewall によってブロックされたという警告が表示される場合があります。外部リソースへのアクセスは既定で制限されるため、これは想定された動作です。必要であれば、[Copilot cloud agent の firewall を設定または無効化][agent-firewall]できます。

1. github.com でリポジトリを開きます。
2. **Pull Requests** を選択します。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment やその他のドキュメントを確認します。具体的な変更内容は異なる場合があります。Exercise 2 の最初に確認した instruction による TSDoc、TypeScript 規約、コメント ヘッダーにも注目します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す表示を探します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks に workflow が追加されます。単一の Astro app のプロジェクト チェックが成功することを確認します。数分かかる場合があります。

## GitHub Copilot に変更を依頼する

チーム メンバーと同じように、PR やコードのインライン コメントで Copilot にメンションして、追加変更を依頼できます。結果は非決定的なため、依頼内容を一律に指定することはできません。たとえば、コード ファイルにヘッダー コメントや TSDoc を追加する、Astro file に TSDoc を追加する、Astro app の構成を説明する README を作成する、といった依頼ができます。

1. PR にコメントを追加し、ユーザーと同じように **@copilot** を付けてドキュメントの変更を依頼します。
2. **View Session** を選択して、Copilot が更新する様子を確認します。
3. **Back to pull request** を選択して PR に戻ります。
4. Copilot の作業完了後に PR に新しい commit が追加されたことを確認します。
5. **Files changed** タブで変更をレビューします。

満足するまで反復できます。変更を続ける場合は、Copilot にコメントで追加の更新を依頼します。レビューの準備ができたら、次の操作を行います。

1. draft PR をレビュー可能な状態にします。
2. `main` にマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue のために Copilot が作成した PR を確認します。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を開きます。
4. **Files changed** タブで生成コードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. workflow の承認待ちを確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![workflow の承認と実行](../../../_images/shared-approve-workflows.png)

8. PR の checks に workflow が追加され、単一の Astro app のチェックが成功することを確認します。
9. **省略可能:** Codespace でこのブランチをチェックアウトし、関連ゲーム機能を手動でテストできます。Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot にコメントで追加変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、custom instruction 更新前に割り当てた issue のコードにコメント ヘッダーや TSDoc を追加したりできます。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ custom agent が実装した機能を確認します。この PR には、Exercise 3 で依頼した高コントラスト モードと、Exercise 4 で追加依頼したライト モードが含まれます。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブで生成コードをレビューします。
5. 次の点に注意して実装を確認します。
   - モードを切り替える user interface (UI) component
   - ユーザー設定を保存する local storage
   - 高コントラストとライト モードの Cascading Style Sheets (CSS) またはスタイル
   - Accessible Rich Internet Applications (ARIA) label やキーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![workflow の承認と実行](../../../_images/shared-approve-workflows.png)

9. PR の checks で単一の Astro app のチェックが成功することを確認します。
10. **省略可能:** Codespace で Copilot が作成したブランチをチェックアウトします。`<branch-name>` をブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

11. アプリを起動し、高コントラストとライト モードの切り替えが動作し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ custom agent は、アクセシビリティのベスト プラクティスに沿うよう Copilot を導きました。改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼します。

## 省略可能な演習 — 作業を委任する

Cloud agent に実際のバックログ item を渡して、自分は別の作業を進める練習をします。

1. issue を作成し、Copilot に割り当てます。たとえば、次の作業を依頼できます。
   - ゲーム詳細ページの backer interest form
   - ゲーム一覧のページネーション
   - データアクセス helper の入力検証とエラー処理

## まとめ

Cloud agent harness を完了しました。

- リポジトリに含まれる custom instruction を確認し、cloud agent の出力への影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境設定、計画、非同期の実行を確認しました。
- アクセシビリティ用 custom agent を作成して使い、高コントラストとライト モードを追加しました。
- Copilot agents page でセッションを監視し、作業中に追加依頼を送りました。
- cloud agent の PR をレビューして反復し、**@copilot** に変更を依頼して workflow を承認しました。

## レビューと次のステップ

Cloud agent harness を完了しました。別の harness に進むと、ここで学んだことを補完できます。

- [VS Code harness](../../vscode/): IDE から Copilot Chat agent mode と MCP 統合を試します。
- [CLI harness](../../cli/): Plan と Autopilot、agent skill、QA custom agent、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済み変更を 2 件公開します。

自分のリポジトリでは、次の作業も試してください。

1. リファクタリングやテスト範囲の issue を cloud agent に割り当てます。
2. 別の分野に特化した custom agent を作成します。
3. 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow に関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall の設定][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
