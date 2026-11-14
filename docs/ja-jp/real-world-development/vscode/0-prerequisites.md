---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動します。エディターで GitHub Copilot Chat が動作することも確認します。

## 演習用リポジトリを設定する

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] を開きます。
2. **Use this template** を選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ template の設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えます。後の演習で参照します。

> [!NOTE]
> **バックログの準備は完了しています**
>
> template からリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使います。自分で issue を作成する必要はありません。

## Codespace を作成する

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタン](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して Codespace を作成します。

   ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかります。サービスを手動でインストールするよりも早く環境を準備できます。その間に GitHub Copilot の機能を確認してもかまいません。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。ローカルで進める場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

VS Code で Copilot Chat agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespaces では通常、すでにインストールされています。

> [!TIP]
> 拡張機能がない場合は [Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、VS Code の Extensions view で **GitHub Copilot Chat** を検索し、**Install** を選択します。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、上部の **Copilot Chat** アイコンを選択します。
3. Copilot Chat で `Hello world` のようなメッセージを入力し、<kbd>Enter</kbd> を押します。
4. サインインを求められた場合は、GitHub アカウントで認証します。
5. Copilot Chat が表示されることを確認します。

   ![Copilot Chat の認証を求める画面](../../../_images/ex1-copilot-authentication.png)

## まとめ

演習用リポジトリを作成し、コード作成に使う Codespace を準備しました。

## 次のステップ

次は Copilot にプロジェクト規約を伝えます。[演習 1 - custom instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template からリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]
- [GitHub Copilot Chat 拡張機能][copilot-chat-extension]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
