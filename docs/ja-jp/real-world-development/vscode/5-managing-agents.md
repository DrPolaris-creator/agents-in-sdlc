---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、codebase の調査、変更案の作成、file の編集、command の実行が自律的に行われます。VS Code の Copilot Chat では、tool call と file 編集をリアルタイムで確認できます。diff を確認して変更を個別に承認または取り消し、追加の prompt で作業を調整できます。editor を離れる必要はありません。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視する。
- 提案された file の編集をレビューして承認する。
- 実行中の session に指示を加え、ハイ コントラストに加えてライト モードの切り替えを追加する。

## シナリオ

[演習 4][previous-lesson] で使ったアクセシビリティ custom agent は、ハイ コントラスト モードを実装しています。作業中にデザイン チームから、既定のテーマが暗すぎると感じる利用者向けにライト モードも追加したいという要望がありました。新しい会話を始める代わりに、既存の session に指示を加えて作業を拡張します。

## Copilot の進捗を監視する

Copilot Chat view には、agent が調べた file、実行した command、提案した編集などの各手順が表示されます。file の編集は diff と **Keep** / **Undo** control とともに chat 内に表示されます。

1. 演習 4 で使った Copilot Chat view に戻ります。
2. agent が作業中に chat をスクロールし、tool call を確認します。file の読み取り、編集、実行許可を求める terminal command が表示される場合があります。
3. agent が file の編集を提案したら、diff を確認します。
   - **Keep** を選択すると変更を承認します。
   - **Undo** を選択すると変更を元に戻し、agent に再度作業させます。
4. chat の上部にある **Files changed** indicator を開くと、session 中に agent が変更した file 全体を確認できます。

> [!NOTE]
> terminal command の実行など、tool call に許可が必要な場合、agent は一時停止して **Allow** prompt を表示します。内容を確認し、許可する command のみ承認してください。

5. agent がハイ コントラスト モードを実装し終えるまで待ちます。完了すると、変更の概要が表示されます。

## ライト モードを追加するよう agent に指示する

ハイ コントラスト モードが実装されたら、同じ会話にライト モードの切り替えを追加します。同じ chat session を続けるため、agent は既存の作業、使用した component、styling、local storage の方法を把握しています。

1. Copilot Chat の入力欄に、次の follow-up prompt を送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent が既存の変更を調べ、ライト モードを追加することを確認します。
3. ハイ コントラスト モードのときと同様に、各編集をレビューします。各 diff で **Keep** または **Undo** を選択します。
4. agent の方針が希望と異なる場合（たとえば toggle UI を意図せず大きく変更する場合）は、chat 入力欄の横にある **Stop** button で停止し、追加の説明を送ります。

> [!TIP]
> 進行中の session に指示を追加すると、最初からやり直すより短時間で調整できます。たとえば、CSS selector を重複させず CSS variable を使う、既存の設定 panel にライト モードを追加して toggle を増やさない、両方の toggle に keyboard shortcut を追加する、などの指示ができます。

## 作業内容をレビューする

commit する前に、agent が変更した file を確認します。

1. VS Code で **Source Control** view を開きます。
2. 変更された file の一覧を確認します。Astro component、style、関連する test が含まれる場合があります。
3. 変更した file をいくつか開き、diff を確認します。custom agent のアクセシビリティ指針（Accessible Rich Internet Applications (ARIA) attribute、keyboard navigation、semantic HTML、local storage による設定保存）が反映されていることを確認します。
4. 問題がなければ、Source Control panel で変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat agent mode を使い、実行中の session を監視して指示を追加しました。file の編集を確認し、承認または取り消し、コンテキストを保持したまま追加の prompt を送って作業を拡張しました。

このレッスンでは、次のことを確認しました。

- agent の作業をリアルタイムで監視する。
- 個々の file 編集を承認または取り消す。
- follow-up prompt で実行中の session に指示を追加する。

次は、このラボで custom agent が生成したアクセシビリティ機能をレビューし、[作業をプル リクエストとして公開します][next-lesson]。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
