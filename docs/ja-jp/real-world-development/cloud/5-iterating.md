---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

このラボでは、ユーザー エクスペリエンスの改善や機能追加など、いくつかのタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザイン チーム向けの関連ゲーム機能、ハイ コントラストとライト モードを含むアクセシビリティ機能です。コードの変更を確認し、必要であれば Copilot にフィードバックを返して改善しましょう。

## 作業をレビューする

生成 AI を使っても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認します。

### シナリオ

GitHub Copilot cloud agent が非同期かつ自律的に作業していても、レビューは必要です。人間が生成されたコードを確認し、通常の開発プロセスを進めます。ここでは、ドキュメント、関連ゲーム、アクセシビリティ機能の提案をレビューしてから、残りのチームにレビューを有効にします。

## GitHub Copilot cloud agent のセキュリティ

cloud agent は非同期かつ監視なしで作業するため、安全性を確保するための制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みアクセスは作業用ブランチに限定されます。
- Cloud agent は GitHub Actions 内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actions workflow を実行するには、人間による承認が必要です。
- [外部 resource へのアクセス][agent-firewall]は、Model Context Protocol (MCP) server を含め、既定では制限されています。

## 生成されたドキュメントをレビューする

最初に、コードにドキュメントを追加するため GitHub Copilot cloud agent が作成したプル リクエスト (PR) を確認します。GitHub.com の通常の PR 画面を使います。

> [!NOTE]
> PR に GitHub Copilot が firewall によってブロックされたという警告が表示される場合があります。既定では Copilot の外部 resource へのアクセスが制限されているため、これは想定された状態です。必要に応じて [Copilot cloud agent の firewall をカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、ドキュメント追加を示すタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、ここまでに学んだことを振り返ったりしてください。

4. PR が準備できたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment などを含むコードを確認します。変更内容は毎回異なります。

   TypeScript の規約、TSDoc doc comment、コメント ヘッダーを確認します。これらは演習 2 の冒頭で確認した custom instruction file に基づいています。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. workflow の実行承認を待っていることを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションに workflow が追加されることを確認します。すべて問題なければ、単一の Astro app の project check が成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードへの inline comment で Copilot に言及できます。Copilot はコメントを受け取り、変更に対応する新しい session を開始します。結果は非決定的なため、依頼する文面を一つに指定することはできません。たとえば、次の変更を依頼できます。

- 各コード file の先頭に、file の役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro file に TSDoc doc comment を追加する。
- Astro app の構造を説明する README を作成する。

1. 上記の例、またはコード内のドキュメントに関する別の提案を使い、**@copilot** をメンションして、生成されたドキュメントの変更をコメントで依頼します。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新のために新しい session を開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しい commit があることを確認します。
5. **Files changed** タブを選択して変更をレビューします。

満足できるまで反復できます。問題がなければ PR を draft から ready に変更し、`main` branch にマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームを示すタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. workflow の実行承認を待っていることを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR の checks セクションに workflow が追加されることを確認します。すべて問題なければ、単一の Astro app の project check が成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこの branch に切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成した branch 名（例: **copilot/fix-8**）に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。先ほどと同様に、コメントを追加して新しい変更を依頼し、cloud agent と反復できます。たとえば表示する関連ゲームの数を調整したり、この作業は custom instruction の更新前に割り当てたため、コメント ヘッダーや TSDoc doc comment の追加を依頼したりできます。**Conversation** タブに新しいコメントを追加すると、Copilot がそれを受け取り、新しい session を開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ custom agent が実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、ハイ コントラスト モードを示すタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 次の項目に特に注意して実装を確認します。
   - 表示モードを切り替える UI component
   - ユーザー設定を保存する local storage の使用方法
   - ハイ コントラスト モードとライト モードの CSS または styling の変更
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) label、keyboard navigation など）
   - モード切り替えを管理する JavaScript/TypeScript code

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. workflow の実行承認を待っていることを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR の checks セクションに workflow が追加されることを確認します。すべて問題なければ、単一の Astro app の project check が成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこの branch に切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成した branch 名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページの再読み込み後も設定が保持されることを確認します。

アクセシビリティ custom agent は、アクセシビリティのベスト プラクティスに沿ってこれらの機能を Copilot が実装するよう導きました。アクセシビリティ上の懸念や改善案がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent にバックログの実作業を任せると、別の作業に集中できます。習慣を身に付けるため、リポジトリに issue をいくつか作成し、Copilot に割り当ててみてください。たとえば次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通して、次のことを行いました。

- この repository に含まれる custom instruction file を確認し、後で cloud agent の出力に反映される様子を確認しました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、自律的な作業を確認しました。
- アクセシビリティ用 custom agent を作成して使用し、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents page を mission control として使い、実行中のアクセシビリティ session を監視して指示を追加しました。
- Copilot が作成したプル リクエストをレビューし、`@copilot` に変更を依頼して workflow を承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学ぶ場合は、ほかのハーネスも試してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot mode、agent skill、custom QA agent、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` bridge を使い、レビュー済みの変更をターミナルから 2 件提供します。

自分の repository で試せる追加のアイデアは次のとおりです。

- refactoring または test coverage の issue を cloud agent に割り当てる。
- 別の領域に custom agent を作成する。
- 異なる stack 用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall を設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
