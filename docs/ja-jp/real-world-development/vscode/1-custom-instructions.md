---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成人工知能（AI）を使うときは、コンテキストが重要です。特定の方法で実施する必要があるタスクや Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。コンテキストを Copilot に渡す方法はいくつかあります。コード生成のガイダンスを与える [instruction file][instruction-files]は、その重要な方法の 1 つです。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認する。
- 現在の命令を使って、フィルタリング用の最初のデータ スライス（publishers helper）を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されるか確認する。
- 次の演習で Copilot が更新済みのガイダンスを使えるように、命令の更新とフィルタリングの作業を `main` に commit する。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させるのではなく、命令を更新した後に振る舞いがどのように変わるかを確認します。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- ユーザー インターフェイス（UI）はダーク モードで、モダンな印象にします。
- TypeScript 用ドキュメント コメント（TSDoc）を使ってコードに説明を追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使って、これらのプラクティスに沿って作業するために必要な情報を Copilot に伝えます。

### カスタム命令

カスタム命令を使うと、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。望ましいコーディング規約、ライブラリ、コメントの種類などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別に適用するタスク単位の命令を作成できます。

Instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。技術スタック、作成中のものの概要、ベスト プラクティス、その他の全体的なガイダンスなど、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などに関するガイダンスを記載します。

> [!NOTE]
> 統合開発環境（IDE）では、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と `applyTo` front matter を含む `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に基づく instruction file][custom-instructions-support]もサポートします。

### Instruction file を管理するためのベスト プラクティス

- `copilot-instructions.md` には、作成中のものの説明、プロジェクト構造、全体的なコーディング標準などのプロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイル種別（単体テスト、Astro コンポーネント、データ レイヤーなど）や特定のタスク向けの命令を記載します。
- 自然な言葉で明確なガイダンスを書き、コードの良い例と避ける例を示します。

Instruction file の作成にも AI の使い方にも唯一の正解はありません。試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットがあると役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けのファイルがあります。
>
> Copilot は instruction file の生成も支援できます。たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` を使います。利用方法は開発環境によって異なります。
>
> テンプレートや出発点には、instruction file やカスタム エージェントなどを集めた [awesome-copilot][awesome-copilot]を利用できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令を確認する

このリポジトリに含まれている instruction file を読みます。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスにも注目します。これらは Copilot とのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドには、命令の適用対象を決めるリポジトリ ルートからの相対 glob が設定されています。この例では、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下まで確認します。`unit-tests.instructions.md` などの instruction file やプロジェクト内のファイルへのリンクがあります。これにより大きな instruction を再利用しやすい単位に分け、コード生成の参考例を示せます。パスはリポジトリのルートではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。後の手順で TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

変更にはブランチを使います。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する*前に* Copilot Chat を使う

カスタム命令の影響を確認するため、まず現在の instruction file のままプロンプトを送信します。後で命令を更新し、同じプロンプトを再実行します。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を開始します。モードとモデルは演習ごとに異なるため、必要な選択は手順内で説明します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. Copilot が helper の配置場所を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント一覧で **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、helper とテストなどのコードを更新する様子を確認します。
6. 提案された helper が、最初の引数に `db` client を受け取り、型付きの publishers 配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。この演習では、命令を更新した後に一貫性が向上することを確認します。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を与えます。コード提案の質を上げるため、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。ここにはコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。
3. 既存の標準のすぐ下に、ファイル コメント ヘッダーと TSDoc コメントの追加を指示する次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file はリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスク向けの `*.instructions.md` のどちらにも作成できます。プロジェクト全体の標準にはリポジトリ レベルのファイルを使います。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys Crowd Funding プラットフォームのパブリッシャー データ アクセス helper です。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * すべてのパブリッシャーの ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns パブリッシャー オブジェクトの配列に解決する Promise。
    */
   ```

次の演習で使えるように、更新したファイルを残しておきます。

## 命令とブランチを commit して push する

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. `git status` を実行し、命令と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. 命令の更新と helper の作業を stage して commit します。`git status` にテストなどの追加ファイルが表示された場合は、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したら、バックログにあるドキュメント標準の Issue に関連付けたプル リクエスト（PR）を作成します。GitHub のツールが接続済みなので、Copilot に Issue を探して PR を作成し、マージするよう依頼できます。

> [!NOTE]
> Copilot に GitHub 操作を依頼するのはこれが初めてです。プロジェクト テンプレートでは、GitHub のツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。後の演習で Model Context Protocol について説明します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント一覧で **Agent** が選択されていることを確認します。
3. instruction file の更新と publishers helper の基盤の両方を説明し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub 操作の **Allow** を求められたら、コマンドを確認して適切な場合に選択します。
5. PR 作成後、次のプロンプトで PR をマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作や shell コマンドの **Allow** を求められたら、内容を確認して適切な場合に選択します。

GitHub Copilot を使って PR を作成し、マージしました。

## まとめと次のステップ

Instruction file から Copilot にコンテキストが伝わる方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- リポジトリの命令とパス単位の instruction file がエージェントに伝わる仕組みを確認しました。
- 現在の命令を使ってフィルタリング用の最初のデータスライスを生成し、結果を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が反映されることを確認しました。
- 次の演習で使えるよう、命令の更新と helper の作業を `main` に反映しました。

次は [agent mode][next-lesson]で、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction file][instruction-files]
- [カスタム命令を作成するベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令をより良く書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
