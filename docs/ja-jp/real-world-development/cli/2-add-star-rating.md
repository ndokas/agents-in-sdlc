---
title: "レッスン 2 - 星評価を追加する: 手軽な改善"
description: "Copilot CLI でゲーム カードを少し変更し、転送ブラウザーで確認して、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲーム データにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで目的を絞った Copilot の会話を始める。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- 転送ブラウザーでアプリを実行し、変更を確認する。
- 最初のプル リクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードにはタイトル、カテゴリ、発行者、説明だけが表示されています。最初の練習として、各カードに既存の評価を表示します。小規模で独立した変更なので、最初のセッションに適しています。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作らず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存済みの会話に戻って同じ話題を続けられますが、ファイルとブランチは通常のディスク上の Git 状態として管理されます。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI を使って新しい機能を追加しました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot CLI で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを調べます。
2. 数値評価がある場合はゲーム カードに表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストで確認されていることを確かめます。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく、確率的に動作します。コードの実装が例と完全に一致することを期待せず、動作をレビューしてください。

## 変更を確認する

コードを読むだけで、動作すると決めつけることはできません。Copilot に Web サイトを起動してもらい、Codespaces が転送するブラウザーで更新後のユーザー インターフェイス（UI）を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能と通知したら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトにマージするため、プル リクエスト（PR）を作成します。

1. 既定のエージェントに、変更を commit するよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR 作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで目的を絞った Copilot の会話を始めました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送ブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は、[フィルタリングの Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
