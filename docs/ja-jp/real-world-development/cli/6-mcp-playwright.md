---
title: "Lesson 6 - Playwright MCP server で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ここまでに説明したように、コードを書く以外にも、データや外部サービスを扱い、Copilot が利用できる自動化を追加する必要があります。ここで Model Context Protocol (MCP) server が役立ちます。MCP server を使うと、Copilot CLI に組み込まれていない tool やサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを理解する。
- まだ利用できない場合は Playwright MCP server を追加する。
- agent にブラウザーを操作させ、フィルタリング機能を確認する。

## シナリオ

unit test と end-to-end test は重要ですが、ユーザー インターフェイス (UI) の変更を検証するには実際に UI を操作する必要があります。Copilot が Web サイトを利用者のように使えるようにし、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部 tool やサービスと通信する方法を提供します。MCP を使うと、AI agent は最新情報を取得し、ユーザーに代わって操作を実行できます。

MCP server は AI agent と外部 tool やサービスの間をつなぎます。各 MCP server は、AI agent が利用できる別々の tool と resource のセットを表します。

よく使われる MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub repository、issue、プル リクエストを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP server があります。GitHub は、エコシステム内の発見やコントリビューションを促進するために [MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP server を追加する

Playwright MCP server をまだ利用できない場合は追加し、Copilot が利用者のように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、ここでの設定手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次の command を入力し、MCP server の追加 dialog を開きます。

   ```plaintext
   /mcp add
   ```

3. name に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type に **STDIO** を選択し、続いて <kbd>Tab</kbd> を押します。
5. **Command** dialog に次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP dialog を閉じます。

## Playwright を使って機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP server を使って同じ確認を行います。

1. 次の prompt を使い、Playwright MCP server で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP server を自分で判断して使うため、明示する必要はありません。ただし使用する server がわかっている場合は、指示すると一貫性が高まり、token の使用量も抑えられます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を確認します。
3. 報告を読み、期待どおりに動作することを確認します。

Copilot は server を起動し、Playwright で Web サイトを操作し、server を停止して結果を報告します。

## まとめと次のステップ

Playwright MCP server を使い、Copilot CLI から実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを学びました。
- まだ利用できない場合は Playwright MCP server を追加しました。
- agent にブラウザーを操作させ、フィルタリング機能を確認しました。

次は、skill とブラウザー tool を専門的な役割にまとめる [QA custom agent][next-lesson] を作成します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
