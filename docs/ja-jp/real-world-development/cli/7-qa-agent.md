---
title: "レッスン 7 - 品質保証エージェントを作成して使用する"
description: "Issue の要件、quality-checks skill、Playwright Model Context Protocol（MCP）を組み合わせる品質保証（QA）カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。ここでは、明確な QA プロセスを定義したカスタム エージェントに、それらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが命令、skill、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選び、フィルタリングの Issue に対する調査結果を確認する。

## シナリオ

Tailspin Toys では、プル リクエスト（PR）を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法で確認したいと考えています。カスタム エージェントは QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルには目的、命令、利用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を説明します。`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらの機能を使って要件を評価し、結果を報告する方法を Copilot に伝えます。QA プロファイルは既存の機能を置き換えず、別の会話を必要としません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルでは、QA が行うチェックと、その作業の境界を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次の依頼を送ります。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと作業範囲を記述できたかを確認します。検証だけを行いたいのに、不完全なエージェントや範囲の広いエージェントが機能を変更することを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. front matter を確認します。`description` は必須です。`name` は任意ですが、指定するとエージェント名が分かりやすくなります。
3. プロファイルの命令を読み、QA が要件から確認を始め、リポジトリの命令に従い、`quality-checks` skill を実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更を commit したり PR を開いたりしないことを確認します。
5. これらの責任や制限が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後で同じフィルタリングの会話を再開し、QA を選ぶと、会話にある Issue と計画時の決定を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトを使って、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始することを確認します。
4. 完了後にレポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その成果を確認しました。このレッスンでは、次のことを行いました。

- カスタム エージェントが命令、skill、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルタリングの Issue に対する調査結果を確認しました。

実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、それらを[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
