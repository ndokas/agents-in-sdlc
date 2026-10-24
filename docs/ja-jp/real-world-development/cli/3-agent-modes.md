---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。より大きな変更には、しっかりしたプロセスが必要です。GitHub Copilot CLI は、組織で既に使われている開発フローに合わせて作業できます。このレッスンから、Issue を起点に機能を作成し、コードの妥当性と動作を確かめ、最終的にプロジェクトへマージする、エージェントを活用した開発プロセスを進めます。

> [!NOTE]
> 機能の開発が終わるまで、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチやプル リクエスト（PR）を分けますが、ここでは中心的な概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub Issue を起点に新しい Copilot 会話を開始する。
- Plan モードで要件を決める。
- Autopilot モードで機能を実装する。
- コードをレビューする。
- 転送ブラウザーで機能を手動検証する。

機能の開発を続けながら、リポジトリの命令を更新し、既存の `quality-checks` skill をカスタマイズし、Model Context Protocol（MCP）による検証を追加して品質保証（QA）エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリと発行者でゲームを絞り込める必要があります。バックログの Issue には機能が説明されていますが、カテゴリを組み合わせる方法など、コーディング前に決める必要のある詳細があります。Plan モードで要件を決めてから、範囲を明確にした実装を Autopilot に任せます。

## 背景

人工知能（AI）コーディング エージェントを開発フローに加えても、基本は変わりません。多くの場合、次のような手順で進めます。

1. 実施内容を記述した Issue から始める。
2. 作成するものの計画を立てる。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新しい機能を手動で確認する。
6. プル リクエスト (PR) を作成する。
7. コード レビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の手順を基にしています。

標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手作業のコードと同じレビュー プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、開発者の入力を待ってから続行します。
- **Plan**: エージェントが先に計画を立てます。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリングの作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub の Issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に作業を任せる前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されることを確認します。
2. 直前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、実装に適した方法を検討して計画を作成します。

3. フィルタリングの動作について質問されたら、希望する動作に基づいて回答します。どの選択肢にも誤りはありません。
4. 計画の作成が終わったら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 自分の決定と一致しない部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたので、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に対する作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の意味の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係の不足やポートの競合が報告された場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. Issue と計画時に決めた内容に照らして、実装を確認します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードを確認できました。次は実際に動くか確かめます。前と同様にアプリを起動し、Codespaces が転送するポートからサイトを開きます。

1. Copilot に次のプロンプトを送り、アプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能と通知したら、**Open in Browser** を選択します。
3. カテゴリ、発行者、および計画で決めた組み合わせで絞り込みを試します。
4. リセットと結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを切り替えながら機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue を起点に新しい Copilot 会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動検証しました。

次は、[カスタム命令を使って][next-lesson]、コードが文書化されたプラクティスに従って生成される方法を詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
