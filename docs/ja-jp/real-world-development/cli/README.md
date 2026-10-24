---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、Model Context Protocol（MCP）ツール、プル リクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証して公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できます。コードベースの調査、コード生成、コマンド実行、外部ツールとの連携をコマンド ラインから行い、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の開発ワークフローを続けて進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして操作に慣れます。
2. 星評価を小さく変更し、ブラウザーで確認して、最初のプル リクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで作成し、Interactive モードでレビューします。
4. リポジトリの命令を更新し、フィルタリング機能に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングの動作を確認します。
7. 品質保証 (QA) カスタム エージェントを作成、選択し、要件、カバレッジ、検証の根拠を確認します。
8. フィルタリングの変更全体をレビューし、Agent Merge で PR をマージします。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。

ワークショップでは PR を 2 つ作成します。1 つ目は星評価、2 つ目は命令の更新、skill の更新、QA プロファイル、テストを含むフィルタリングです。フィルタリングと品質ワークフローは、同じ会話とブランチで進めます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、操作に慣れる |
| [2. 星評価を追加する: 手軽な改善][ex2] | 最初の変更 | 既存の評価と評価なしの場合の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue に基づいて機能を計画し、Autopilot で作成して Interactive でレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認、更新し、フィルタリングに適用する |
| [5. quality-checks skill のカスタマイズと利用][ex5] | 繰り返し可能なチェック | 既存 skill を確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI に MCP を設定し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成、選択し、最終検証の根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを振り返る |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続する |

## 前提条件

ワークショップを始める前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
