---
title: "Lesson 4 - Custom instruction で Copilot を導く"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が参照できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、コードの構成方法も指定できます。フィルタリング機能を作成したので、Copilot が使った instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction と、パスを限定した instruction file が agent にどのように渡るかを確認する。
- コーディング標準を守るため、instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントには、コードを言い換えるのではなく、意図や自明でない決定を記述する。
- `db/` と `src/lib/` の export function には、`db` 引数を注入できる場合はそれも含め、目的、引数、戻り値を説明する TSDoc/JSDoc を追加する。
- 再利用可能な Astro component には `Props` の契約を記載し、関連コードが変更されたらコメントを最新の状態に保つ。
- 既存の format と lint のガイダンスを維持する。

instruction file を使うと、これらのプラクティスに沿って作業するための情報を Copilot に渡せます。

## Instruction file

Custom instruction は、コーディング スタイルや要件などのコンテキストや設定を Copilot に渡すために使います。好みのコーディング規約、使用するライブラリ、追加したいコメントを指定できます。リポジトリ全体に適用する instruction と、特定のファイル種別に適用する instruction の両方を作成できます。

instruction file は次の 2 種類です。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` file は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの instruction format が各環境でどの程度サポートされるかは異なります。特定の形式を使う前に、[custom instruction のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには instruction file が用意されています。変更を加える前に、既存の内容を確認します。

1. Codespace に戻ります。
2. Codespaces editor で（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この instruction は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` folder を開き、内容を確認します。Astro file、Drizzle データ レイヤー、test などの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instruction の適用先を決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内の file へのリンクを確認します。これにより、instruction を小さな再利用可能な単位に分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて instruction を更新する

既存の file はよい出発点ですが、まだ不足があります。中心となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc comment を追加する標準を定めます。

1. `.github/copilot-instructions.md` を開き、**Code formatting guidance** セクション（35 行目付近）を探します。
2. 最後の箇条書きとして次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話を開始するときにリポジトリの instruction を読み込みます。instruction を保存した後、フィルタリングの会話を終了して再開します。そうすることで、機能のコンテキストを保ちながら更新後のガイダンスを読み込めます。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリの root で、`game-filters-cli` branch から保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` branch が checkout されていることを確認します。
3. 更新したガイダンスを適用するよう、Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript file を確認します。新しい TSDoc comment があり、コードの説明として正確であることを確認します。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する方法を確認し、新しい標準をフィルタリング機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction と、パスを限定した instruction file が agent にどのように渡るかを確認しました。
- コーディング標準を守るため、instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、lint と test を一貫して実行できるよう、再利用可能な `quality-checks` skill をカスタマイズして実行します。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [Custom instruction のサポート状況][custom-instructions-support]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
