---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を加えて、プル リクエストを作成します。その間、別の作業を続けられます。

この演習では、まず cloud agent が従う custom instruction を追加し、次に GitHub issue を割り当てて実装を依頼します。custom agent を確認して使用し、agents dashboard からセッションを監視して指示を追加します。最後に、cloud agent が作成したプル リクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Custom instruction][ex1] | コンテキスト | cloud agent が従う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. Custom agent][ex3] | 専門エージェント | custom agent を確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視し、指示を追加する |
| [5. 反復][ex5] | レビュー | プル リクエスト (PR) をレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウントがある
- [ ] アカウントまたは organization で Copilot cloud agent が有効になっている

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free では利用できません。Copilot Business または Enterprise の場合は、organization の管理者が有効にする必要があります。

> [!NOTE]
> Model Context Protocol (MCP) はこのハーネスでは扱いません。Copilot で MCP server を使う方法については、[CLI ハーネス](../cli/) または [VS Code ハーネス](../vscode/)を参照してください。

## 開始する

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
