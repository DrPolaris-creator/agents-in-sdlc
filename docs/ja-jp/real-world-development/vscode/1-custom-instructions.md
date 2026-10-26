---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、Copilot がそのガイダンスを参照できるようにします。Copilot に特定のコンテキストを渡す方法はいくつかあります。特に重要なのは [instruction file][instruction-files] です。instruction file で、コード生成のガイダンスを指定します。

この演習では、次のことを行います。

- リポジトリの custom instruction とパスを限定した instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように渡るかを確認する。
- *現在の* instruction を使って、フィルタリングの最初のデータ スライス（publishers helper）を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される標準を追加する。
- 同じ prompt を再実行し、生成されたコードに新しい標準が反映されることを確認する。
- instruction の更新とフィルタリングの作業を `main` に commit し、次の演習で更新後のガイダンスを使えるようにする。

> [!CAUTION]
> 生成されたコードが設定した標準から外れることがあります。Copilot は非決定的です。この演習の目的は、instruction の更新後に振る舞いがどう変化するかを確認することであり、出力を文字単位で一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず unit test が必要です。
- ユーザー インターフェイス (UI) はダーク モードで、現代的な印象にする必要があります。
- コード内に TSDoc doc comment を使ってドキュメントを追加します。
- 各 file の先頭に、file の役割を説明するコメント ブロックを追加します。

instruction file を使って、これらのプラクティスに沿って作業するために必要な情報を Copilot に渡します。

### Custom instruction

Custom instruction は、コーディング スタイルや要件などのコンテキストや設定を Copilot に渡し、より適切な提案やコードを得るために使います。コーディング規約、使用する library、追加したいコメントの種類などを指定できます。repository 全体に適用する instruction と、特定の file 種別に適用する instruction を作成できます。

instruction file は次の 2 種類です。

- `.github/copilot-instructions.md` は、repository への**すべて**のリクエストで Copilot に送られる単一の instruction file です。技術スタック、開発中のものの概要、ベスト プラクティスなど、多くの chat や CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` file は、特定のタスクや file 種別向けに作成できます。TypeScript や Astro のような言語、UI component の作成、unit test の追加などに関するガイダンスを記載します。

> [!NOTE]
> 統合開発環境 (IDE) で作業している場合、instruction file は Copilot Chat でのコード生成にのみ使用されます。コード補完や next-edit suggestion には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時に repository レベルの instruction file と `applyTo` frontmatter がある `*.instructions.md` file の両方を使用します。
>
> Copilot は [ほかの標準を使った instruction file][custom-instructions-support] もサポートしています。AGENTS.md や CLAUDE.md も利用できます。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成について詳しく説明することは、このワークショップの範囲外です。ここでは、サンプル プロジェクトにある代表的な方法を紹介します。

- `copilot-instructions.md` には、開発中のもの、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` file は、file 種別（unit test、Astro component、データ レイヤー）や特定のタスク向けの instruction に使います。
- 自然な言葉でガイダンスを明確にし、望ましいコードと望ましくないコードの例を示します。

instruction file に唯一の正解はありません。AI の使い方と同様に、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file を用意します。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの file があります。
>
> Copilot に instruction file の作成を依頼することもできます。各 surface で手順は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`）。必要な場合は、使用中の surface のレッスンで手順を説明します。
>
> template や出発点を探す場合は、instruction file、custom agent、その他のリソースが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

この repository に含まれる instruction file を確認します。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` file があります。editor または GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスにも注目します。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` folder を開き、内容を確認します。Astro file、Drizzle データ レイヤー、test などに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instruction の適用先を決める glob（repository root からの相対パス）です。この例では `**/*.test.ts` に一致する TypeScript test file が対象になります。
5. このプロジェクトで unit test を作成するときの instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` など、他の instruction file や既存 file へのリンクに注目してください。大きな instruction を小さく再利用可能な単位に分け、Copilot がコード生成時に参照できる例を示します。（記載されたパスは repository root ではなく、この instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準がありますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc doc comment と file コメント ヘッダーのルールを追加します。

## 変更用ブランチを作成する

変更作業のベスト プラクティスに従い、作業用ブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を開始します。mode と model は演習によって異なります。必要な選択は各手順で説明します。

custom instruction の効果を確認するため、まず現在の instruction file のまま prompt を送信します。その後で instruction を更新し、同じ prompt を再送信して違いを確認します。

1. 前の演習で開いた editor tab を閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、helper の配置先を Copilot に知らせます。
3. Chat view の agent picker で **Agent** を選択し、Copilot が file を変更できるようにします。

   ![Chat view の agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送ります。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、通常は helper file と test file を含むコード変更を適用することを確認します。
6. 提案された helper が型付き関数であり、最初の引数として `db` client を受け取り、型付き publishers 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc doc comment と file レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加することがあります。その場合も問題ありません。instruction を更新した後に一貫性が向上することが重要です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を渡す file です。コード提案を改善するため、repository のコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。既存のコーディング標準はありますが、コード内ドキュメントのルールがないため、生成した helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、全体的なガイダンス向けの repository レベル（`.github/copilot-instructions.md`）にも、特定の言語、file 種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment ルールのようなプロジェクト全体の標準には、repository レベルの file が適しています。

## prompt を再実行して変化を確認する

instruction に doc comment のルールを追加したため、先ほど生成した publishers file を更新するよう Copilot に依頼します。同じ標準がコードの更新方法を導きます。

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しい file に集中できるようにします。
3. 前と**同じ prompt** を送ります。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しい標準に従わせ、次の演習で使用する実際のコードに適用できました。

## instruction と branch を commit して push する

instruction file もほかの asset と同様、ソース管理で扱います。instruction と helper の作業を repository に保存します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. instruction の更新と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper の作業で Copilot が変更した file を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. branch を repository に push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

branch を push したら、バックログにあるドキュメント標準の issue と関連付けたプル リクエストを作成します。GitHub tool がすでに接続されているため、Copilot に issue を探し、issue を完了する PR を作成してマージするよう依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは初めてです。プロジェクト template には GitHub tool が設定されています。Copilot が最初に tool を使うとき、VS Code で GitHub へのサインインを求められる場合があります。prompt に従って認証してください。後の演習で、この接続に使われる Model Context Protocol を扱います。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat view を開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file 更新に関連する issue を探し、現在の branch から PR を作成するよう Copilot に依頼します。PR には issue を完了することと、今後のフィルタリング作業用に publishers helper の基盤を追加したことを記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot から GitHub 操作の **Allow** を求められた場合は、command を確認し、適切な場合に **Allow** を選択します。
5. PR が作成されたら、Copilot に PR をマージしてローカルの branch を `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot から GitHub 操作や shell command の実行許可を求められた場合は、内容を確認して **Allow** を選択します。

GitHub Copilot の支援を使ってプル リクエストを作成し、マージしました。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の* instruction を使ったコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じ prompt を再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- instruction の更新と helper の作業を `main` に commit し、次の演習で使えるようにしました。

次は、[agent mode][next-lesson] で instruction を使い、複数の file にまたがる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot の instruction file][instruction-files]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人向け custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
