---
title: "Lesson 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の開発ワークフロー全体に取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルタリング issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリング issue から作業を始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- custom instruction で agent を導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) custom agent を作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの証拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge によるマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認しました。

## 作成したもの

このワークショップには、更新済みの `main` から分岐した、それぞれ別のブランチで進める 2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な表示をゲーム カードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA profile と関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、一連の作業を 1 つの PR にまとめています。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP を通じて Copilot が行うブラウザー操作という複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、その結果と要件、テスト範囲のレビューをまとめました。

追加するテストでは、本当に不足している範囲を補ってください。QA の実行で新しいテストが不要という結果になることもあります。マージを許可する前に、コードと検証の証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に渡すコンテキストや tool は、作業結果に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA profile を作成し、MCP server を構成しました。これらのカスタマイズは会話をまたいで再利用し、チームの要件に応じて調整できます。instruction は標準を設定し、skill は繰り返し行うタスクを記述し、custom agent は専門的な役割を定義し、MCP server は外部 tool に接続します。agent の要約だけでなく、実際の変更と tool の結果をレビューしてください。

タスクに合わせて **モードとモデル** を選びます。実装前に方針を検討するには **Plan**、範囲を絞った変更を対話しながら進めるには **Interactive**、明確に範囲を定めたタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

基盤と同様、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、結果が大きく変わります。

## さらに試す

中心的なワークフローを完了しました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` を使って、code review agent に変更の分析を依頼する。
- `/rubber-duck` を使って、問題を説明し、別の視点を得る。
- `/fleet` を使って、独立したサブタスクを並列に調整する。
- `/worktree` を使って、別のタスクを分離する。
- `/delegate` を使って、タスクを Copilot cloud agent に送る。

## 次のステップ

どのツールも、使い続けることで活用方法が身につきます。本番コード、趣味のコード、長い間考えていた小さなアプリなど、さまざまな作業で試してください。学びをチームと共有し、チームからも学んでください。ドキュメントも確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
