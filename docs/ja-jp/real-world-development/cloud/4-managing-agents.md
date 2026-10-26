---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直近の演習では、ユーザー エクスペリエンスを改善し、機能を追加する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub の [agents page][agents-page] など、cloud agent に割り当てたタスクを管理するツールが複数あります。この mission control では、オープンなプル リクエスト (PR) がある agent のタスクをすべて確認できます。実行された処理を調べ、進行中のセッションに追加の指示を与えることもできます。

このレッスンでは、次のことを行います。

- agents page で cloud agent のタスクを監視する。
- 実行中のセッションに指示を追加し、機能を拡張する。

## シナリオ

エージェントにハイ コントラスト モードを作成させた後、チームはライト モードも追加するとよいと気づきました。サイトのスタイルと切り替え機能を更新しているため、同時に実装するのが適切です。ハイ コントラスト モードに加えてライト モードも作成するよう、agent の作業に指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents page](https://github.com/copilot/agents) に移動します。
2. メイン ペインと左ペインにあるタスク一覧を確認します。次のタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次の内容が含まれる場合があります。
   - リポジトリを checkout する。
   - Copilot の環境を作成する。
   - Model Context Protocol (MCP) server を設定する。
   - タスクを完了するための各種操作を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示される PR ペインも確認します。ここから PR と変更ファイルを確認し、さらに監視できます。

## Cloud agent に指示を追加する

アクティブなタスクを確認したので、agent がハイ コントラスト モードを実装している間に、ライト モードの切り替えも含めるよう依頼します。

1. ハイ コントラスト モードのタスクに対応するセッションを選択します。Copilot が付けた名前や作業状況により、タイトルは異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すまで数分間セッションを確認します。次のようなメッセージが表示されたら開始しています。
3. **Steer active session while Copilot is working** ダイアログに、次の prompt を入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のセッションを steer するダイアログが強調表示された agents page のスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が prompt を受け取り、作業計画に加えたことを確認します。

## Copilot の作業を続ける

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していた手順を完了した後、新しい依頼を作業に反映します。

今回も数分かかるため、ここまでに学んだことを振り返る時間にできます。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、指示を追加するための中心的な場所である Copilot agents page を確認しました。mission control では、進行中と完了済みのタスクを追跡し、作業内容をレビューして、実行中のタスクの方向を変えられます。

次の内容を確認しました。

- mission control と agents page で cloud agent のタスクを監視しました。
- 実行中のセッションに指示を追加し、機能を拡張するよう依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプル リクエストの[反復作業を行います][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
