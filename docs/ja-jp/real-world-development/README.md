---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を段階的に計画、実装、テストし、プル リクエストをレビューして、エージェントと作業を管理します。

## シナリオ

あなたは、開発者向けテーマのボード ゲーム クラウドファンディングを運営する架空の企業、Tailspin Toys に新しく加わった開発者です。チームのバックログは GitHub issue として登録済みで、すぐに取り組めます。バックログには、フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、開発者が作業する場所で利用できます。作業スタイルに合った環境を選んでください。各ワークショップでは、それぞれのセットアップから始め、共通の Tailspin Toys シナリオに取り組みます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を進めるよう指示し、進捗を監視して、エージェントが作成したプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、Model Context Protocol (MCP) server、custom agent を利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
