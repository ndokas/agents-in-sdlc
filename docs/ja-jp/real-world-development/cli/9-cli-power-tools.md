---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデルの選択、セッション共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作用のスラッシュ コマンドが用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話コンテキストを管理する方法を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の中心的なワークフローを終えたので、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使って、自分で内容を確認したり圧縮したりすることもできます。

1. Codespace に戻ります。Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令・ツール・メッセージ・空き領域に分かれたコンテキストの内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。`/compact` を使うと、圧縮するタイミングを選べます。無関係な別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、人工知能（AI）ツールの使い方を改善できます。`/share` コマンドを使うと、セッションを Markdown または HyperText Markup Language（HTML）ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合う保存先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並行または委任する

ワークショップの中心的な作業は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分け、並列実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に渡します。cloud agent は非同期で作業し、プル リクエストを開く場合があります。

これらのコマンドを使うと、新たな worktree やリモート作業が作成される場合があるため、任意の手順としています。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく知りたい場合は、[cloud agent のワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使い、設定、セッション共有、内部情報の確認を行いました。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で会話コンテキストの管理方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。ここまでの作業を[振り返り、次の学習方法を確認します][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
