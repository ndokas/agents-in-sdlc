---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成人工知能（AI）を使うときは、コンテキストが重要です。作業の進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。[Instruction file][instruction-files]を使うと、何をするかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス単位で適用される instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認する。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用される新しい標準を追加する。

> [!NOTE]
> VS Code や CLI の環境とは異なり、この演習では変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、影響をリアルタイムで比較しにくいためです。追加した instruction file の効果は、この環境の後半で cloud agent が作成したプル リクエストをレビューするときに確認できます。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには常に単体テストを作成する。
- ユーザー インターフェイス（UI）はダーク モードで、モダンな印象にする。
- TypeScript 用ドキュメント コメント（TSDoc）を使ってコードに説明を追加する。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加する。

Instruction file を使い、これらのプラクティスに沿って作業するために必要な情報を Copilot に伝えます。

### カスタム命令

カスタム命令を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解しやすくできます。望ましいコーディング規約、ライブラリ、含めたいコメントの種類などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用するタスク単位の命令を作成できます。

Instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の instruction file です。技術スタック、作成中のものの概要、ベスト プラクティス、その他の全体的なガイダンスなど、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別に利用できます。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> 統合開発環境（IDE）では、instruction file は Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と `applyTo` front matter を含む `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に基づく instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成に関する詳しい説明はこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイル種別（単体テスト、Astro コンポーネント、データ レイヤーなど）や特定のタスク向けの命令を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例、避ける例を示します。

Instruction file の作成にも、AI の使い方にも、唯一の正解はありません。試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットがあると役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot は instruction file の生成も支援できます。利用方法は各環境で異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。必要な場合は、その環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令を確認する

このリポジトリに含まれている instruction file を確認します。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスにも注目してください。これらは Copilot とのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction file を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの相対 glob で適用対象を指定します。この例では、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するための命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、一番下まで確認します。`unit-tests.instructions.md` などの instruction file や、プロジェクト内の既存ファイルへのリンクがあります。これにより大きな instruction を再利用しやすい単位に分け、コード生成時に参考にする例を Copilot に示せます。パスはリポジトリのルートではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次に、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 新しいリポジトリ標準を追加する

ここでは、コード内のドキュメントを TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーで記載するルールを追加します。後の演習で cloud agent が Issue に取り組む際に、この標準を参照します。

編集を始める前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから命令を読み取りますが、変更を整理してコミットするためにブランチを使います。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準が記載されていますが、コード内ドキュメントの規則はまだありません。
4. 既存の標準のすぐ下に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで見たように、instruction file はリポジトリ全体向けの `.github/copilot-instructions.md` と、言語、ファイル種別、タスク向けの `*.instructions.md` のどちらにも作成できます。ドキュメント コメントの規則のように、プロジェクト全体に適用する標準はリポジトリ レベルのファイルに記載します。

## 命令の更新をコミット、push、マージする

cloud agent は、Issue が対象とするブランチの instruction file を読み取ります。次の演習で Issue を割り当てると、cloud agent は `main` からブランチを作成するため、追加した命令を `main` に反映しておきます。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成し、マージします。次の演習で cloud agent が Issue に取り組むとき、追加した命令を参照できるようになります。

> [!TIP]
> このワークショップでは、`main` に直接変更を commit してもかまいません。その場合はブランチの手順を省略できます。ここでは実際のプロジェクトに近い方法としてブランチを使っています。

## まとめと次のステップ

Instruction file から Copilot にコンテキストが伝わる仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この標準が、以降の演習で cloud agent が作成するプル リクエストに反映されます。

次は、[GitHub Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction file][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令をより良く書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
