---
title: "Lesson 7 - 品質保証 (QA) agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次は、これらの機能を、明確な QA 手順を備えた custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する方法を確認する。
- 再利用可能な品質保証 (QA) profile を作成して確認する。
- QA agent を選択し、フィルタリング issue に照らした所見をレビューする。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、test coverage、ブラウザーでの動作を一貫して確認したいと考えています。custom agent を使うと、この QA の手順を調整し、再利用可能なレポートを作成できます。

## Custom agent とは

custom agent は、Markdown profile で定義する Copilot の専門的な役割です。profile には、agent の目的、指示、使用できる tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA role を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明し、`quality-checks` skill は再利用可能なチェックをまとめます。Playwright MCP はブラウザー tool を提供します。QA profile は、要件を評価して所見を報告するために、それらをどのように使うかを Copilot に指示します。それぞれを置き換えるものではなく、別の会話も必要ありません。

## QA profile を作成する

機能の PR を開く前に、再利用可能な QA profile の作成を Copilot に依頼します。profile には、QA が実行する確認事項と守るべき境界を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive mode であることを確認します。
2. default agent に、次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Profile を確認する

新しい agent を使う前に、Copilot が意図した QA workflow と境界を profile に反映したか確認します。確認だけを求めているのに、agent が不完全または広すぎる指示で機能を変更することを防げます。

1. `/diff` を入力し `.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は任意ですが、追加すると表示名が明確になります。
3. profile の指示を読み、QA が要件から確認を始め、リポジトリの instruction に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit したりプル リクエストを開いたりしないことを確認します。
5. これらの責務や境界が不足している場合は、続行する前に default agent に修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話を開始するときにプロジェクトの agent を読み込みます。profile を作成した後、同じフィルタリングの会話を再開し、QA を選択します。これにより、issue と計画時の決定事項を使えます。

1. 次の prompt で QA agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能な agent の一覧に表示されない場合があります。表示されなくても、上記の command で有効にできます。

2. 次の prompt で、QA agent に機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を始めることを確認します。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用できる専門的な役割をワークフローに加え、その作業をレビューしました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP tool と連携する方法を確認しました。
- 再利用可能な QA profile を作成して確認しました。
- QA agent を選択し、フィルタリング issue に照らした所見をレビューしました。

実装、skill の更新、QA profile、test、検証レポートが揃いました。次は、それらをまとめて機能の PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
