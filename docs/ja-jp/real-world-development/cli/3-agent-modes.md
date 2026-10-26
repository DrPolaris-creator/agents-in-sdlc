---
title: "Lesson 3 - Agent mode: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルタリング機能を実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しました。より大きな変更には、さらに確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて使えるため、正しいものを適切な方法で作成できます。このレッスンから、issue を使って機能を作成し、コードを確認し、期待どおりに動作することを検証して、プロジェクトにマージするという、agent を活用した一般的な開発プロセスを進めます。

> [!NOTE]
> 機能のワークフローを続ける間は、同じ会話とブランチを使います。実際にはファイル種別ごとに別のブランチや PR を使うこともありますが、ここでは中心的な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の会話を開始する。
- Plan mode で要件を定める。
- Autopilot mode で機能を実装する。
- コードをレビューする。
- 転送ブラウザーで機能を手動検証する。

この機能開発を続ける中で、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、Model Context Protocol (MCP) による検証を追加し、QA agent を作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のゲーム カタログが増え、利用者がカテゴリや publisher でゲームを絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリの組み合わせなど、実装前に決める詳細があります。Plan mode で方針を決めてから、範囲を定めた実装を Autopilot に任せます。

## 背景

AI coding agent を開発フローに加えても、ソフトウェア開発の基本は変わりません。むしろ、基本の重要性が高まります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した issue から始める。
2. 実装内容を計画する。
3. コードを作成してレビューする。
4. test を実行して検証する。
5. 新機能を手動で検証する。
6. プル リクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 実際の詳細はチームや組織によって異なりますが、多くの場合は上記の流れに沿って進めます。

この標準的な方法に従うと、AI が生成したコードも要件を満たし、手書きのコードと同じ検証を経ていることを確認できます。

## 会話モード

**会話モード**は、agent にどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: agent が変更を提案し、次に進む前に開発者の入力を待ちます。
- **Plan**: agent が先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: agent がコードを書き、test を実行し、タスクが完了するまで自律的に反復します。

最初に Plan mode で計画を確認し、その後 Autopilot で実装します。

## issue から開始する

フィルタリングの作業を始める前に、Codespace とリポジトリの準備をします。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

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
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** という issue を選びます。
9. <kbd>C</kbd> を押して issue を prompt に追加し、**Session** タブに戻ります。

prompt の先頭に `#7` または同様の番号が追加されたことを確認します。`#` を使うと、GitHub 上の issue またはプル リクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能を実装する前にアプローチを決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan mode に切り替え、計画の作成を Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。prompt の下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue 参照の後に、次の prompt を入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適なアプローチを検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望する実装に合わせて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画全体を確認します。
6. 希望と異なる箇所があれば、修正を依頼します。

## Autopilot を承認する

計画を確認したら、Copilot に実装を任せます。Autopilot は、タスクが完了したと判断するまで繰り返し作業します。

1. **Accept plan and build on autopilot (recommended)** または、インストール済みバージョンで同様の意味を持つ選択肢を選びます。
2. prompt の下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成して test を実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。まず計画をレビューしてください。Copilot が依存関係の不足や port の競合を報告した場合は、チェックが完了したと判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないか検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルタリングの実装と test を確認します。
3. 結果を issue と計画時の決定事項に照らして確認します。
4. コードを確認したら、<kbd>Esc</kbd> を押して diff 表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がないことを確認したら、Codespaces の転送 port を使ってアプリを起動し、Web サイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces が port `4321` を利用可能と表示したら、**Open in Browser** を選択します。
3. カテゴリ、publisher、および計画で決めた組み合わせで絞り込みを試します。
4. reset と結果が空の場合の動作が、issue と計画での決定に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した dev server を停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の会話を開始しました。
- Plan mode で要件を定めました。
- Autopilot mode で機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動検証しました。

次は、custom instruction を使ってコード生成を導き、文書化されたプラクティスに沿っていることを確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
