---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instruction][previous-lesson] |
|:--|

多くの organization は、未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望など、技術的負債を抱えています。GitHub Copilot cloud agent は、コードの更新や機能追加などの作業を自律的に実行できます。作業が完了すると、人間の開発者がレビューできる draft プル リクエスト (PR) を作成します。これにより、時間のかかる作業を委任し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コードを生成する環境をカスタマイズする。
- 操作が安全に行われるようにする。
- issue の範囲を明確に定める重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys は、技術的負債に取り組みたいと考えています。最初のサイトを作成した委託業者はドキュメントを残していません。最初の作業として、アプリケーションのすべての export function に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに同じカテゴリの関連ゲームを表示し、利用者が引き続きゲームを探せるようにします。まだ完成度の高い実装は必要なく、ユーザー エクスペリエンス (UX) の受け入れテストに使える簡単な実装で十分です。現在はブロッカーですが、優先度の高い issue がほかにあります。

このように、優先順位が下がりやすい作業は cloud agent に任せられます。cloud agent が非同期で作業する間、開発者は別の仕事に集中できます。その後で戻り、作業をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業します。たとえば [GitHub issue を Copilot に割り当て][assign-issue]ることができます。割り当てると、Copilot は進捗を追跡する draft プル リクエストを作成し、環境を準備して作業を開始します。作業中または完了後にセッションを確認できます。提案された解決策をレビューできる状態になると、プル リクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作も、詳しく見ると複雑な場合があります。[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI pair programmer として、段階的に進め、学び、試し、調整します。生成 AI を追加しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

これまでの演習で、custom instruction が Copilot を導く方法を確認しました。custom instruction の演習を済ませている場合は、`.github/copilot-instructions.md` の効果をすでに確認しています。まだの場合は、ここで簡単に確認できます。cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction file を読み取り専用で確認し、後でその影響を見分けられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で、次の file を開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc doc comment の要件に注目します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter に注目します。この instruction は `**/*.test.ts` file に適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てた後、作成されるプル リクエストで TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認します。後の演習でもう一度レビューします。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するには、通常、必要な開発 tool と依存関係を使える環境を準備します。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニア (SWE) と同じような方法で作業します。

cloud agent は GitHub Actions を使って作業環境を準備します。作業開始前に実行する [setup workflow][setup-workflow] を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発 tool と依存関係を利用できるようになります。ラボを円滑に進めるため、この workflow は事前に設定されています。Node.js、プロジェクトの依存関係、end-to-end test 用のブラウザー、単一の Astro app 用に移行と seed を済ませた SQLite database が準備されます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は操作に独自の token を使います。
    permissions:
      # 依存関係のインストールなどでセットアップ中に repository を clone する場合は contents: read 権限が必要です。clone しない場合、手順の完了後に Copilot が自動で clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app のセットアップ - Node.js（現在、app 全体は Astro + Drizzle/libSQL です）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # build と test に必要なデータを用意するため、ローカル SQLite database を migrate して seed します。
      - name: Set up the database
        run: npm run db:setup
```

通常の GitHub workflow と同じ形式ですが、重要な点があります。

- `copilot-setup-steps` という job が 1 つあります。この job は Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger があり、リポジトリの **Actions** タブから workflow を手動で実行できます。Copilot が使うまで待たずに、workflow が正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は知られていますが、多くのプロジェクトでは情報が古いか、ドキュメントが不足しています。このような技術的負債は放置されがちで、生産性を下げ、コードの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成にも役立つため、この issue を cloud agent に割り当てます。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエストを開きます。タイトルは `[WIP]: Code lacks documentation` のような形式です。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot が todo list を作成するまで数分かかる場合があります。Copilot は環境を準備し、前述の workflow を実行し、プロジェクトを分析して、問題への取り組み方を決めています。

13. todo list を確認し、Copilot が実行するタスクを確認します。
14. プル リクエストの timeline を下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot session を表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. live session を確認します。Copilot はコードを調べ、状態を把握し、計画を検討して決定した後、コードを作成します。

これには数分かかります。cloud agent の主な利点は、別の作業をしている間に非同期でタスクを進められることです。この機能を活用し、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チームのために関連ゲームのセクションを追加する作業を agent に任せ、別のタスクを進められます。cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことがわかります。

![Copilot が issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同様に作業するため、常に監視する必要はありません。完了してからレビューできます。次は custom agent の作成と利用に進みます。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を使いました。cloud agent に issue を割り当て、非同期に実行させることができます。技術的負債への対処、新機能の作成、ある framework から別の framework へのコード移行などに利用できます。

次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- issue の範囲を明確に定める重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、次は custom agent を作成して使用します。[Copilot cloud agent は Model Context Protocol (MCP) server も使用できます][cloud-agent-mcp]。前の演習で確認した custom instruction も利用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent の setup workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
