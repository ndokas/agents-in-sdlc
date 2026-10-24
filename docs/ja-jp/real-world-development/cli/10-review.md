---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト（PR）、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の開発ワークフローをひととおり進めました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを調査して、あらかじめ登録されたフィルタリングの Issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリングの Issue を起点に、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを誘導し、既存の `quality-checks` skill をカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを試しました。
- 品質保証（QA）カスタム エージェントを作成、選択し、要件、skill の結果、テスト カバレッジ、ブラウザーでの根拠を確認しました。
- フィルタリングの変更全体をレビューし、Agent Merge にフィルタリング PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップでは、更新した `main` から作成した別々のブランチで、2 つの PR を完了しました。

1. **星評価:** ゲーム カードに既存の `starRating` と、評価がない場合の明示的な状態を表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## 検証の種類

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` skill は単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はその結果を要件とテスト カバレッジのレビューに組み合わせてから、PR に進みました。

追加するテストは、実際に不足しているカバレッジを補う必要があります。QA の実行で新しいテストが不要になることもあります。マージを許可する前に、コードと根拠をレビューし、変更後は影響を受ける根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、命令を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。命令は標準を設定し、skill は繰り返し可能な作業をまとめ、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけではなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討、**Interactive** は作業に参加しながら進める小規模な変更、**Autopilot** は範囲が明確なタスクに適しています。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。作成するもの、理由、方法を明確に伝えると、出力が大きく変わります。

## さらに試す

中心的なワークフローを完了しました。Copilot CLI には、次のような機能もあります。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を整理し、別の視点から意見を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別の作業を分離する。
- `/delegate` で Copilot cloud agent にタスクを委任する。

## 次のステップ

どのツールも、使い続けることで使い方が上達します。業務コードや個人のコードに使い、長く作りたいと思っていた小さなアプリにも試してください。チームと学んだことを共有し、お互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境を試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
