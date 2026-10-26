---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフロー内の特定のタスクや領域に合わせた人工知能 (AI) assistant を作成できます。repository の `.github/agents` folder にある Markdown file で agent を定義し、対象の作業を効果的に進めるための集中的な指示、ベスト プラクティス、コーディング パターン、ドメイン固有の知識を Copilot に渡します。チームの専門知識を、Web Content Accessibility Guidelines (WCAG) 準拠を確認するアクセシビリティ agent、安全なコーディングに従う security agent、一貫した test pattern を維持する test agent など、再利用可能な agent として定義できます。

custom agent は、プロジェクトの `.github/agents` folder またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には、少なくとも `name` と `description` を含む YAML frontmatter と、agent の振る舞い、専門性、指示を定める Markdown prompt が含まれます。

### Custom agent と agent skill の比較

custom agent と [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown file で定義し、AI に作業方法を伝えます。簡単に区別すると、**custom agent は作業者、skill は tool** です。

custom agent には独自の context window があり、作業中に skill や他の agent を調整できます。このラボでは、アクセシビリティ custom agent がガイドラインに沿ってサイトをレビューし、更新します。その一環として、pull request workflow の skill や test の実行・管理用 skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成方法に唯一の「正解」はありません。AI を使う他の作業と同様、環境やシナリオに合う方法を見つけるため、テストと反復を行います。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダーク テーマは文字と背景のコントラストが不十分で読みにくいという意見が寄せられました。デザイン チームは、利用者が切り替えられるハイ コントラスト モードの実装を求めています。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認する。
- Copilot Chat でアクセシビリティ agent を使い、ハイ コントラスト モードを実装する。

アクセシビリティは重要なため、機能をできるだけ早く実装します。ここでは custom agent に機能の生成を依頼します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot の作業を導く方法を理解するため、内容を確認します。

まず、`main` からアクセシビリティ作業用の新しい branch を作成します。フィルタリング PR は演習 3 の branch に分けておきます。

1. codespace に戻り、ターミナルを開いて次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` field を含む YAML frontmatter を確認します。

   > [!CAUTION]
   > `name` と `description` を含む frontmatter は custom agent に必須です。

4. 続いて、次の内容を示すセクションを確認します。
   - アクセシブルな Web サイトのコードを生成する際の主要な責務。
   - アクセシビリティのベスト プラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat で custom agent を使う

VS Code では、`.github/agents` に定義された各 custom agent が Copilot Chat view の agent picker に表示されます。custom agent を選ぶと、その agent の指示と tool に合わせて chat session が設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択します。mode と model は演習によって異なり、必要な選択は各手順で説明します。

1. Chat view の agent picker で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. chat view 下部の agent picker（既定では **default** と表示）を選択します。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次の prompt をアクセシビリティ agent に送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が codebase を調べ、変更を提案し、プロジェクト file を編集することを確認します。各編集は、file path とレビュー可能な diff とともに chat に表示されます。
6. 編集が追加されるにつれ、agent が変更した file の一覧を示す **Files changed** indicator が更新されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は実際の repository を変更し、Astro component、CSS、関連 test などの既存 file を編集します。

次の演習では、作業中の変更をレビューして指示を追加します。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents] は、特定のタスクや領域に特化した AI assistant です。custom agent を使うと、チームの専門知識や標準を再利用可能な agent にして、Copilot の作業を導けます。

次の内容を確認しました。

- custom agent の定義方法。
- Copilot Chat agent mode で custom agent を使う方法。

次は、agent の作業を[監視して指示を追加し][next-lesson]、変更を確認しながら同じ session にライト モードの切り替えを追加します。

## リソース

- [Custom agent について][custom-agents]
- [統合開発環境 (IDE) で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の設定][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
