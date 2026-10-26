---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI を使って Tailspin Toys の 2 つの変更をビルド、検証、提供しながら、Copilot CLI の mode、customization、MCP tool、pull request の自動化を学びます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys に機能を追加する一連のワークフローに取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして操作に慣れます。
2. 星評価の小さな変更を加えてブラウザーで確認し、最初のプル リクエスト (PR) を手動でマージします。
3. フィルタリングの issue から始め、Plan mode でアプローチを決め、Autopilot mode で実装し、Interactive mode でレビューします。
4. リポジトリの instruction を更新し、フィルタリング作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) custom agent を作成し、要件、テスト範囲、検証の証拠をレビューします。
8. フィルタリングの変更全体をレビューし、Agent Merge で PR を処理します。
9. コンテキスト、モデル、共有、任意の cloud agent への委任に使える slash command を確認します。

このワークショップでは、PR を 2 件作成します。1 件目は星評価、2 件目は instruction の更新、skill の更新、QA profile、テストを含むフィルタリング機能です。各ツールを試す間、フィルタリングと品質ワークフローは同じ会話とブランチで進めます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI をインストールする][ex1] | インストール | Copilot CLI をインストールして認証し、操作に慣れる |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージする |
| [3. Agent mode: Plan と Autopilot][ex3] | Agent mode | issue から機能を計画し、Autopilot で実装して Interactive mode でレビューする |
| [4. Custom instruction で Copilot を導く][ex4] | コンテキスト | instruction を確認して更新し、フィルタリングに適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA agent を作成して使う][ex7] | 要件とテスト範囲 | 専門的な profile を作成して選択し、最終的な検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | 振り返り | ワークフロー、再利用可能なカスタマイズ、参考資料を確認する |
| [オプション: Foundry を取り入れる][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいた agent をデプロイして Web サイトに接続する |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## 始める

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
