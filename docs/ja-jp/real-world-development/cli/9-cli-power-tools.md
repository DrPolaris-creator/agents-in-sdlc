---
title: "Lesson 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト、モデル、セッション共有を管理する slash command と、cloud agent への任意の委任について学びます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様、GitHub Copilot CLI にも操作に使える slash command が用意されています。高度な機能や内部状態の確認、追加の設定オプションを利用できます。ここまでは `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使って、利用できるモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認する。

## シナリオ

CLI の中心的なワークフローが完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに近づくことがあります。Copilot CLI は必要に応じて会話を自動で compact します。slash command を使うと、状態の確認や手動での compact もできます。

1. Codespace に戻ります。Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、system instruction、tool、message、空き領域の内訳を確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合、大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI は、ウィンドウがいっぱいになるとコンテキストを自動で compact します。タイミングを自分で選ぶ場合は `/compact` を使います。関係のないタスクに移り、新しい会話を始める場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに強みが異なり、開発者によって好みも異なります。Copilot CLI では、利用するモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択します。一覧を閉じるには <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストールされているバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、`/share file` によるローカルの Markdown エクスポートなど、目的に合った出力先を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、prompt、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並行化または委任する

中心的なワークショップの作業は完了しました。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` は独立したサブタスクを subagent に割り当て、並列で実行できます。
- `/worktree` は別のタスク用に分離された Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期で作業させます。プル リクエストが作成される場合があります。

これらのコマンドは、追加の worktree やリモート作業を発生させることがあるため任意です。試す前に、新しい、明確な範囲のタスクを開始し、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定、セッションの共有、内部状態の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

ほかにも多くの slash command があり、Copilot CLI にはまだ試せる機能があります。次は、[ここまでに学んだことを振り返り][next-lesson]、学習を続けるための手順を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
