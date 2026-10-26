---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

agent mode でフィルタリング機能を作成しました。プル リクエスト (PR) を開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに **Playwright Model Context Protocol (MCP) server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、branch を公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に tool を追加する方法を学ぶ。
- workspace に Playwright MCP server を追加する。
- Copilot にブラウザーを使ったフィルタリング機能の手動テストを依頼する。
- branch を公開して、フィルタリング作業のプル リクエストを作成する。

## Model Context Protocol (MCP) とは

agent mode は editor の外にある tool に接続すると、さらに多くの作業を実行できます。Model Context Protocol (MCP) は、Copilot がそのような操作を行うための標準です。agent が外部 tool やサービスと通信できます。

![agent mode が context、大規模言語モデル (LLM)、MCP server と VS Code extension の tool を含む tool と連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI agent は外部 tool やサービスと通信できます。MCP により、最新の情報（resource）にアクセスしたり、ユーザーに代わって操作（tool）を実行したりできます。

MCP server は AI agent と外部 tool やサービスの間をつなぎます。既存のアプリケーション プログラミング インターフェイス (API) や NPM package などのローカル tool との通信を管理します。各 MCP server は、AI agent がアクセスできる別々の tool と resource のセットを表します。

よく使われる MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository を管理する API に接続します。AI agent は repository の作成や更新、issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。AI agent は Web ページへの移動、form の入力、button の選択などを実行できます。

ほかにも、多様な tool と resource に接続する MCP server があります。GitHub は、エコシステム内の発見やコントリビューションを促進するために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの観点から、MCP server はプロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP server のみを使用し、機密性の高い resource や操作へのアクセスを許可するときは慎重に判断してください。

## MCP の設定を確認する

`.vscode/mcp.json` file には、この VS Code workspace で利用できる MCP server が設定されています。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` server がすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` entry はプロジェクト template に含まれています。前の演習で Copilot が backlog issue を読み取れたのはこのためです。[remote GitHub MCP server][remote-github-mcp-server] を使うためローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が最初に tool を使うときに GitHub で認証します。

## Playwright MCP server を追加する

2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が操作できるブラウザーを利用でき、機能をテストできます。

1. `.vscode/mcp.json` で `github` entry と並ぶ `playwright` entry を追加します。file は次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. file を保存します。

remote `github` server と異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag により Playwright は画面を表示せずにブラウザーを実行します。desktop がない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys は end-to-end test に Playwright を使っているため、必要なブラウザーは通常インストール済みです。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行してください。

## Server を起動して信頼する

VS Code は MCP server を**必要になったときに**起動します。Copilot が最初に server の tool を必要とすると、VS Code は server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` entry の上に表示される **Start** を選択して server を起動し、設定が正しいことを確認します。
2. VS Code から server を信頼するか確認された場合は、設定をレビューして、server を起動できるよう信頼します。

手動での起動は任意です。省略した場合、次の手順で Copilot が tool を必要としたときに VS Code が server を起動し、信頼の確認を求めます。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。agent はページを開き、移動し、filter を適用し、結果を読み取って報告します。

Playwright MCP server はスクリーンショットではなく、ページの [accessibility tree][playwright-mcp-server] を使って動作します。agent は支援技術と同じように、button、link、list item など、構造化されラベルの付いた要素を扱います。そのため、機能の簡単な確認がアクセシビリティの基本的な確認にもなります。

server が接続され、アプリが実行中になったら、Copilot にフィルタリング機能を試すよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と報告内容を照らし合わせます。問題があれば追加で質問するか、プル リクエストを開く前にコードの修正を依頼してください。

> [!NOTE]
> test 中はアプリが `http://localhost:4321` で動作している必要があります。dev server を停止した場合は、prompt の送信前に起動します。Copilot が初めて Playwright MCP server を使うときにブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## branch を公開してプル リクエストを作成する

機能が動作することを確認できたら、チームがレビューできるようプル リクエスト (PR) を作成します。最初に `filtering-vscode` branch を公開します。

1. codespace の **Source Control** panel を開き、Copilot が行った変更を確認します。
2. **+** icon を選択して変更を stage します。
3. **Sparkle** button を選択し、commit message を生成します。

   ![変更を表示する Source Control panel のスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して branch を repository に push します。

## プル リクエストを作成する

プル リクエストは github.com や GitHub command-line interface (CLI) から作成できます。ここでは GitHub Copilot に作成を依頼し、関連する issue と結び付けます。

1. Copilot Chat panel に移動し、**New Chat** を選択して新しい session を開始します。
2. agent picker で **Agent** が選択されていることを確認し、Copilot が GitHub tool を使えるようにします。
3. 次の prompt で PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作に必要な処理を Copilot に許可します。GitHub tool を初めて使うときに認証を求められる場合があります。画面の指示に従って認証します。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が表示する link を選択して PR を確認します。まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に tool を追加する方法を学びました。
- workspace に Playwright MCP server を追加しました。
- Playwright MCP server を使い、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- branch を公開して、フィルタリング作業のプル リクエストを作成しました。

MCP server を使って機能を*テスト*しました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティ レビューなどの作業に使う [custom agent][next-lesson] を作成します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
