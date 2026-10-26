---
title: "演習 1 - Custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにしておきます。[Instruction file][instruction-files] を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパスを限定した instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように渡るかを確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後の prompt を比較しません。Copilot cloud agent は GitHub issue に対して非同期に作業するため、リアルタイムで並べて効果を示すのが難しいからです。後の演習で cloud agent が作成したプル リクエストをレビューするときに、instruction file の影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- データ レイヤーには必ず unit test が必要です。
- ユーザー インターフェイス (UI) はダーク モードで、現代的な印象にする必要があります。
- コード内に TSDoc doc comment を使ってドキュメントを追加する必要があります。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加する必要があります。

instruction file を使うと、これらのプラクティスに沿って作業するために必要な情報を Copilot に伝えられます。

### Custom instruction

Custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件をより正確に理解させられます。コーディング規約、使用するライブラリ、追加したいコメントなどを指定できます。リポジトリ全体に適用する instruction と、特定のファイル種別に適用するタスク レベルの instruction の両方を作成できます。

instruction file は次の 2 種類です。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。技術スタック、作成中のものの概要、ベスト プラクティスなど、ほとんどの chat や CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に作成できます。TypeScript や Astro などの言語向けのガイドラインや、UI component の作成、unit test の追加といったタスク向けのガイダンスを記載できます。

> [!NOTE]
> 統合開発環境 (IDE) で作業している場合、instruction file は Copilot Chat でのコード生成にのみ使用されます。コード補完や next-edit suggestion には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と `applyTo` front matter を含む `*.instructions.md` file の両方を使用します。
>
> Copilot は [ほかの標準を使った instruction file][custom-instructions-support] もサポートしており、AGENTS.md や CLAUDE.md も利用できます。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。ただし、サンプル プロジェクトに含まれるファイルは代表的な方法を示しています。

- `copilot-instructions.md` には、作成中のものの説明、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` file は、ファイル種別（unit test、Astro component、データ レイヤー）や特定のタスク向けの instruction に使用します。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

instruction file に唯一の正解はありません。AI の使い方と同様に、試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file が必要です。このプロジェクトには、ユーザー インターフェイス (UI) の更新や [Astro][astro-instructions] など、さまざまなタスク向けの file があります。
>
> Copilot に instruction file の作成を依頼することもできます。各 surface で手順は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`）。必要な場合は、使用中の surface に対応するレッスンで扱います。
>
> template や出発点を探す場合は、instruction file、custom agent、その他のリソースが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリには、中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` file があります。エディターまたは GitHub Web UI で開いて内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスにも注目してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` folder を開いて内容を確認します。Astro file、Drizzle データ レイヤー、test などに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instruction が適用されるファイルを決める glob（repo root からの相対パス）です。この例では、`**/*.test.ts` に一致する TypeScript test file が対象です。
5. このプロジェクトで unit test を作成するときの instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存 file へのリンクに注目してください。大きな instruction を小さく再利用可能な file に分け、Copilot がコード生成で参照できる例を示せます。（記載されたパスは repo root ではなく、この instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ標準を追加する

次に行う編集は、TSDoc doc comment とファイル レベルのコメント ヘッダーを追加するプロジェクト全体のルールです。cloud agent は、後の演習で割り当てる issue に取り組む際に、このルールを参照します。

編集する前に、作業用ブランチを用意します。cloud agent は issue の対象ブランチにある instruction を読み取りますが、変更を整理して commit するため、ここではブランチを作成します。

1. codespace のターミナルで、ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を渡す file です。コーディング標準を記載して、コード提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション（27 行目付近）を探します。既存のコーディング標準に、コード内ドキュメントのルールがまだないことを確認します。
3. 既存の標準のすぐ下に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、グローバルなガイダンス向けのリポジトリ レベル（`.github/copilot-instructions.md`）にも、言語、ファイル種別、タスクを限定する `*.instructions.md` にも作成できます。今回追加した doc comment ルールのようなプロジェクト全体の標準には、リポジトリ レベルの file が適しています。

## instruction の更新を commit、push、merge する

cloud agent は、issue の対象ブランチにある instruction file を読み取ります。次の演習で Copilot に issue を割り当てると、cloud agent は `main` からブランチを作成します。そこで instruction の変更を `main` に取り込み、cloud agent が使えるようにします。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成し、マージします。次の演習で Copilot に割り当てる issue に対して、cloud agent がこの instruction を読み取れるようになります。

> [!TIP]
> このワークショップではブランチを使い、実際のプロジェクトに近い流れにしています。ワークショップで `main` に直接作業する場合は、ブランチ作成の手順を省略し、`main` に直接 commit できます。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。以降の演習で cloud agent が作成するプル リクエストに、この標準が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
