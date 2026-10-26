---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に環境を準備します。Tailspin Toys repository のコピーを作成し、作業用の [codespace][codespaces] を起動して、editor で GitHub Copilot Chat が利用できることを確認します。

## ラボ用 repository をセットアップする

作成するコード用の repository を用意するため、[template][template-repository] からインスタンスを作成します。新しいインスタンスにはラボに必要な file が含まれ、演習を通して使用できます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. ラボ repository のページで **Use this template** を選択します。次に **Create a new repository** を選択して、自分用の repository を作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる organization に repository を作成します。

    ![repository template の設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成した repository のパス（**organization-or-user-name/repository-name**）をメモします。ラボの後半で参照します。

> [!NOTE]
> **バックログの準備は完了しています**
>
> template から repository を作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこれらの issue を使います。自分で issue を作成する必要はありません。

## Codespace を作成する

次に、ラボの演習に使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードを作成、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、extension、tool を使える、機能が充実した統合開発環境 (IDE) を利用できます。

1. 新しく作成した repository に移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して codespace を作成します。

    ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするより短時間で済みます。作成を待つ間に、次に扱う GitHub Copilot の機能を確認できます。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ひとまず、ブラウザーのタブを閉じずに開いたままにします。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらにも必要な前提条件がインストールされています。ローカルで実行する場合は、クローンした repository を Visual Studio Code (VS Code) で開き、プロンプトが表示されたら **Reopen in Container** を選択します。codespace と同じ dev container が作成されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat agent mode を使うには、IDE に GitHub Copilot Chat extension が必要です。GitHub Codespace を使っている場合、通常はすでにインストールされています。

> [!TIP]
> GitHub Copilot Chat extension がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または Visual Studio Code の Extensions view を開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

extension をインストールした後、GitHub アカウントで認証する必要がある場合があります。

1. codespace に戻ります。
2. editor の右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** icon を選択します。
3. Copilot Chat window に `Hello world` のようなメッセージを入力し、Enter を押します。これで Copilot Chat が有効になります。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の手順に従って認証します。

    ![Copilot Chat の認証 prompt の例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat window が表示されることを確認します。

## まとめ

ラボ用 repository のコピーを作成し、コードを書くときに使う codespace の作成を開始しました。

## 次のステップ

次は Copilot を使って作業します。プロジェクトの規約を伝える [演習 1 - Custom instruction][next-lesson] に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
