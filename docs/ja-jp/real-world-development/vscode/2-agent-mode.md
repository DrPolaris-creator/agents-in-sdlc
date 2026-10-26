---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数の file の変更や test の実行が必要です。開発者は、必要な file を探し、変更し、test を実行し、問題を調べ、更新漏れの file を見つけて再度変更する、といった作業を行います。

このような作業に GitHub Copilot Agent Mode を使えます。

Copilot Agent Mode は IDE 内で自律的に作業します。既存のプロジェクト構造を調べ、必要な変更を行い、test を実行し、問題が見つかった場合は修正します。Agent Mode を使って Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。agent mode は IDE 内で作業し、すばやく対話できます。cloud agent は issue やプル リクエストを介してやり取りし、チーム メンバーのように非同期で作業します。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定して、複数の file にまたがる変更を行う方法。
- GitHub Copilot Agent Mode でユーザー インターフェイス (UI) とデータ レイヤーに機能を実装する方法。
- Copilot Agent Mode が生成した変更と test を、コードベースにマージする前にレビューする方法。

## シナリオ

ゲーム一覧が増えたため、利用者がカテゴリや publisher で絞り込めるようにします。前の演習で publishers helper を追加しました。ここでは、Copilot Agent Mode でデータ レイヤー、UI、test の残りの作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、現在の機能を確認するため Tailspin Toys Web サイトを開きます。

この Web サイトは、開発者向けテーマのボード ゲームを扱うクラウドファンディング プラットフォームです。ゲーム一覧と詳細情報を表示します。単一の Astro app であり、ページは build 時に静的な HyperText Markup Language (HTML) として生成されます。ページは frontmatter から Drizzle ORM を使ってローカル SQLite database を直接参照します。別個の backend アプリケーション プログラミング インターフェイス (API) や client-side ユーザー インターフェイス (UI) framework はありません。再利用可能なデータ アクセス helper は `src/lib/` にあり、対話機能には標準の Document Object Model (DOM) API を使った小さな Astro `<script>` を使います。

### Web サイトを起動する

`npm run dev` task を使って Astro dev server を起動します。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習を終えていれば `main` branch にいます。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. 新しい branch を作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次の command で Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねる dialog が表示されたら、**x** を選択して閉じます。

5. ターミナルに表示された Astro server の `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは codespace の Web server に接続する private tunnel です。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **Home Page**: ボード ゲームの一覧と、タイトル、画像、説明を表示します。
- **Game Details Page**: ゲームを選択すると、そのタイトル、説明、publisher、カテゴリなどを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択します。mode と model は演習によって異なり、必要な選択は各手順で説明します。

Web サイトの初期実装は動作していますが、新しい機能を追加したいと考えています。まず、template からプロジェクトを作成したときに自動で作られた backlog issue を Copilot に表示してもらいます。

1. Chat view の agent picker で **Agent** を選択します。Agent は自律的に計画し、複数 file を変更し、ターミナル command や tool を実行します。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能な model の一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定の model を推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、比較的一貫した体験を得られるよう手順に記載しています。大規模言語モデル (LLM) は確率的に動作するため、結果が手順と異なることがあります。これは正常で、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP command を使う場合がありますが、それでもタスクを完了できます。

3. 次の prompt を送って、repository の backlog を Copilot に確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって最も有用と考える項目に基づいて優先順位を付けたことを確認します。

## Instruction file を確認する

Copilot にコードを生成させる前に、作業のコンテキストを渡すための instruction file を確認します。Web サイトに機能を追加する方法が記載された [user interface (UI)][ui-instructions] file を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の全体的なガイダンスを確認します。内容には次のものがあります。
   - architecture の概要。
   - component 設計、testability、accessibility の原則。
   - Astro、Tailwind CSS、Drizzle など、file 種別ごとの instruction への link。

> [!TIP]
> instruction file から、別の instruction file とプロジェクト内の file の両方を参照できます。path は instruction file の場所からの相対パスです。これにより、instruction を再利用できる単位に分け、複雑な内容を整理し、例や template を示せます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくともデータ レイヤー、test、ゲーム一覧ページの UI を更新します。すべての test が成功してからマージする必要があります。これらの作業を Copilot Agent Mode に依頼します。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instruction file を追加します。

   ![UI instruction file を選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat view の agent picker で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. model が **Claude Sonnet 4.5** のままであることを確認します。
5. backlog の関連 issue に基づいて機能を実装するよう、次の prompt を送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関係する file（データ レイヤー helper、UI、test）を探す様子を確認します。その後、file を変更して test を実行します。

   ![Copilot がプロジェクト file を調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、file を変更し、test を実行します。作業の複雑さや codebase によって数分かかることがあります。作業中、editor に **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot に確認されたら **Continue** を選択して test を実行します。

   ![Copilot Chat pane が test 実行の確認を求める dialog のスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理が一時停止したり、途中で test が失敗したりすることがあります。Copilot はコード生成と test を繰り返し、エラーが検出されなくなるまで修正します。

   ![Copilot Agent Mode の完了した chat session を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat window で **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加しました。次のことを確認しました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーに機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定して、複数の file にまたがる変更を行う方法。
- Copilot Agent Mode が生成した変更と test を、コードベースにマージする前にレビューする方法。

次は、[Playwright MCP server で機能をテスト][next-lesson]し、プル リクエストを作成します。

### 追加演習 — ページングを実装する

ゲーム一覧が増えた場合は、ページングが必要になります。この演習で学んだことを使って、ページングを実装するよう Copilot に依頼してみてください。検討できる点は次のとおりです。

- 既存の instruction file を使うなど、現在のベスト プラクティスに従う。
- 利用者が page size を選べるようにするか、固定値にするかを決める。
- 希望する実装に必要なガイダンスを prompt に含める。
- Copilot に変更を依頼し、コンテキストを追加しながら反復する。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
