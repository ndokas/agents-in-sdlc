---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズしてフィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発にはさまざまな作業があります。ここまでに、コードの動作を手動で確認し、instruction file でコーディング標準に従うようにしました。テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には **agent skill** が適しています。skill は、これらの操作を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認する。
- 結果のレポート形式をカスタマイズする。
- skill を再読み込みして実行する。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` skill を使っています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 命令、スクリプト、リソース

Agent skill は、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースをまとめ、必要に応じてエージェントに読み込ませます。基本的には、skill 名のフォルダーにある `SKILL.md` で構成されます。この Markdown ファイルには、skill の名前と説明を定義する front matter、概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、skill が実行時に使うスクリプトやほかのリソースを含むサブフォルダーを追加することもできます。

> [!NOTE]
> skill に追加のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクト既存のコマンドを使うため、`SKILL.md` だけで構成されています。

skill は、チームで共有、再利用するリポジトリの `.github/skills` フォルダー、またはユーザーの `~/.copilot/skills` フォルダーに配置できます。

## skill を確認する

Tailspin Toys のチームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` skill を確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は、Copilot がいつ skill を呼び出すか判断するために使われます。
3. 命令を読み、テストと lint の実行方法がどのように説明されているか確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot CLI から直接、または自然な言葉で skill を呼び出せます。Copilot に 3 つのチェックを実行してもらいます。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールがどのような結果を報告したかを明確にするため、skill を更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に、次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されます。

## 更新した skill を実行する

Copilot CLI は会話を再開せずに、編集した skill を再読み込みできます。変更を確認します。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。

- 単体テスト、lint、型チェックを行う `quality-checks` skill を確認しました。
- 結果のレポート形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更はフィルタリング機能のプル リクエスト（PR）に含まれます。次は [Playwright Model Context Protocol（MCP）サーバー][next-lesson]を使い、Copilot がサイトを直接操作できるようにします。

## ほかの skill の例

以下のコミュニティの例は参考情報であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
