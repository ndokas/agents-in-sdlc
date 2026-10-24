---
title: "レッスン 8 - 機能のプル リクエストを作成してマージする"
description: "フィルタリング、命令、skill の更新、品質保証（QA）プロファイル、テストをまとめてレビューし、プル リクエスト（PR）を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、命令の更新、skill の更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。すべてをまとめてレビューし、プル リクエスト (PR) を作成します。星評価の PR は手動でマージしましたが、今回は **Agent Merge** にマージの手続きを任せます。

> [!NOTE]
> 通常は、機能、命令の更新、skill の更新、QA エージェントをいくつかの PR に分けます。このワークショップでは簡略化のため、フィルタリングと品質チェックの作業を 1 つの会話とブランチで行い、1 つの PR にまとめます。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学ぶ。
- 機能全体の変更と検証の根拠を確認する。
- フィルタリングの PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリング機能の作業を通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、マージの承認を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション (CI) チェック、レビュー コメントへの対応、必要な場合の rebase など）を処理します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、開発者が **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に変更をレビューし、マージ可能な状態か判断してください。

## Agent Merge で PR を管理する

すべてのコードを作成したので、変更をレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可される手順に従ってレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの手続きなど、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
