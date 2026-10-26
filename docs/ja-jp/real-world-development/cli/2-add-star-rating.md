---
title: "Lesson 2 - 星評価を追加する: 手軽な改善"
description: "Copilot CLI でゲーム カードを小さく変更し、転送ブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに最初の変更を加えます。変更範囲は小さくします。ゲームのデータには星評価がすでにありますが、ホーム ページのゲーム カードにはまだ表示されていません。agent に評価を表示するよう依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を始める。
- agent に小さな変更を依頼する。
- `/diff` で変更をレビューする。
- 転送ブラウザーでアプリを実行し、変更を確認する。
- 最初のプル リクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかしホーム ページのゲーム カードには、タイトル、カテゴリ、publisher、説明しか表示されません。まずは各カードに既存の評価を表示します。最初のセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app と異なり、通常の CLI 会話では、ターミナルで checkout しているリポジトリと Git ブランチを使用します。専用の worktree は作成されません。保存した会話に戻って同じ議論を再開できますが、ファイルとブランチは通常のディスク上の Git 状態です。

会話には、prompt と agent の応答、ファイルを調査・編集する agent の tool 操作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を開始して変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot がまだ起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を使って <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次の prompt で変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われるファイルを探して、必要なコードを作成します。Copilot CLI で新しい機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はマージ前にレビューします。Copilot CLI で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲーム カードに数値評価がある場合は表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態も test で確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して diff 画面を閉じます。

> [!NOTE]
> 生成 AI と同様、Copilot は確率的に動作するため、コードは例と異なることがあります。特定の実装との一致ではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces が転送するブラウザーでユーザー インターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces が port `4321` を利用可能と表示したら、**Open in Browser** を選択します。
3. ゲーム カードに 5 点満点の評価が表示されることを確認します。
4. Copilot に戻り、起動した server を停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。次はプル リクエスト (PR) を作成し、コードをプロジェクトにマージします。

1. default agent に、変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、PR を開きます。
4. 変更されたファイルとチェックの結果をレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- 目的を絞った Copilot の会話を機能ブランチで開始しました。
- agent にゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送ブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は、フィルタリング issue から始め、Plan モードと Autopilot モードを使って、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
