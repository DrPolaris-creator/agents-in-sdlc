---
title: "演習 3 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフローの特定のタスクや領域に合わせた人工知能 (AI) assistant を作成できます。リポジトリの `.github/agents` folder にある Markdown file で agent を定義し、対象の作業をより効果的に進めるための集中的な指示、ベスト プラクティス、コーディング パターン、ドメイン固有の知識を Copilot に渡します。チームの専門知識を、Web Content Accessibility Guidelines (WCAG) 準拠を確認するアクセシビリティ agent、安全なコーディングに従うセキュリティ agent、一貫した test pattern を維持する test agent など、再利用可能な agent として定義できます。

custom agent は、プロジェクトの `.github/agents` folder またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には、少なくとも `name` と `description` を含む YAML frontmatter と、agent の振る舞い、専門知識、指示を定める Markdown prompt が含まれます。

### Custom agent と agent skill の比較

custom agent と [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown file で定義され、AI に作業方法を伝えます。最も簡単な区別は、**custom agent は作業者、skill は tool** ということです。

custom agent には専用の context window があり、作業を進めるために skill や他の agent を調整します。このラボでは、アクセシビリティ custom agent がガイドラインに照らしてサイトをレビューし、更新します。その作業の中で、プル リクエスト ワークフローの skill や test を実行、管理する skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成方法に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合う方法を見つけるためにテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、custom agent を使って次のことを行います。

- custom agent の定義方法を確認する。
- タスクを custom agent に割り当てる。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザーから、現在のダーク テーマは文字と背景のコントラストが不十分で読みにくいという意見がありました。この問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を求めています。

アクセシビリティは重要なため、機能をできるだけ早く実装します。ここでは custom agent を使って機能を作成します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。Copilot の作業をどのように導くかを理解するため、その内容を確認します。

codespace に戻り、アクセシビリティ custom agent file を確認してください。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` field を含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は custom agent に必須です。

3. 続いて、次の内容を示すセクションを確認します。
   - アクセシブルな Web サイトのコードを生成する際の主要な責務。
   - アクセシビリティのベスト プラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Custom agent に issue を作成して割り当てる

Mission control は、環境内のすべての agent を扱う場所です。Copilot cloud agent にタスクを割り当て、監視し、必要に応じて方向を変えたり追加の指示を渡したりできます。まず、ハイ コントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![custom agent と Accessibility が強調表示された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域に特化した AI assistant である GitHub Copilot の [custom agent][custom-agents] を確認しました。custom agent を使うと、チームの専門知識や標準を再利用可能な agent として定義し、Copilot が特定の作業を効果的に進めるよう導けます。

次の内容を確認しました。

- custom agent の定義方法。
- タスクを custom agent に割り当てる方法。

Copilot がハイ コントラスト モードを実装している間に、[mission control から agent セッションを監視して指示を追加します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [organization で custom agent を使う準備][org-custom-agents]
- [enterprise で custom agent を使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
