---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成人工知能（AI）を使うときは、コンテキストが重要です。特定の方法で作業する必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[Instruction file][instruction-files]は、必要なコードだけでなく、その構成方法も説明します。フィルタリング機能を作成したので、Copilot が使った命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス単位の instruction file がエージェントに伝わる仕組みを確認する。
- コーディング標準を適用するために instruction file を更新する。
- instruction file がコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインがあります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明する。
- `db/` と `src/lib/` のエクスポート関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で記述する。`db` を注入する引数がある場合は、それも説明する。
- 再利用可能な Astro コンポーネントは `Props` の契約を説明し、関連コードの変更時にコメントを最新の状態に保つ。
- 既存の書式設定と lint のガイダンスを維持する。

Instruction file を使い、これらのプラクティスに沿って作業するために必要な情報を Copilot に伝えます。

## Instruction file

カスタム命令を使うと、コンテキストや設定を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。望ましいコーディング規約、ライブラリ、含めたいコメントの種類などを指定できます。リポジトリ全体に適用する命令と、特定のファイル種別やタスクに適用する命令を作成できます。

Instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイル種別、タスクに関するガイダンスを提供します。

> [!NOTE]
> ほかの instruction 形式とサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令を確認する

スターター プロジェクトには、いくつかの instruction file があらかじめ含まれています。変更を加える前に、既存の内容を確認します。

1. Codespace に戻ります。
2. ターミナルではなく Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの説明とコーディング ガイダンスを確認します。これらの命令は、このリポジトリにおける Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの instruction file を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドで命令を適用するファイルを glob 指定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクト内の既存ファイルへのリンクを確認します。大きな命令を再利用できる小さなファイルに分割し、Copilot に参考例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存ファイルは良い出発点ですが、追加できるルールがあります。中核となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントが付くようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の行をセクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを保持したまま、新しいガイダンスを読み込むため、命令を保存した後にフィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルタリングの会話を選び、`game-filters-cli` ブランチにいることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正しく説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、新しい標準をフィルタリング機能に適用しました。

- リポジトリの命令とパス単位の instruction file がエージェントに伝わる仕組みを確認しました。
- コーディング標準を適用するため instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、lint とテストを一貫して実行できるよう、再利用可能な [quality-checks skill をカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
