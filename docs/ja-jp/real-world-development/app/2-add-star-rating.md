---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットを試したので、**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。変更は小規模です。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエスト（PR）としてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を確認する。
- 小規模で対象を絞った変更をプロジェクトに加えるようエージェントに依頼する。
- ワークスペースの差分ビューで変更をレビューする。
- アプリをローカルで実行し、ブラウザーで変更を確認する。
- 最初の PR を開いてマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。最初の練習として、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションに適した、小さく自己完結した変更です。

## セッションの構造

**セッション**とは、分離された独自のワークスペースで実行されるエージェントとの会話です。各セッションには**専用の Git worktree とブランチ**が割り当てられるため、変更を競合させずに、あるセッションで機能を追加し、別のセッションでバグを修正するといった作業を並行して行えます。セッションはリポジトリごとにグループ化されてサイドバーに表示されます。いずれかを選択して切り替えます。

セッションには、エージェントとの**会話**、ファイルの調査や編集に伴うエージェントの**ツール アクティビティ**、差分を確認できる**変更済みファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

プロジェクトを調べて機能を実装するため、新しいセッションを開始します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用の新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択します。次のプロンプトで変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、Copilot にプロンプトを送信します。

Copilot app は、最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調査し、新機能に必要なファイルを特定してコードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

人工知能（AI）が生成した変更は、小さなものでもマージ前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。数値評価がある場合は表示し、`starRating` が `null` の場合は `No rating yet` を表示する、次のような小さなブロックが含まれるはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は、ほかの生成人工知能（AI）ツールと同じく確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、おおむね同じ内容になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントによる自動チェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示がテストされていることを確認してください。前提条件の不足やチェックのスキップは成功を意味しません。インストールの要求は内容を確認してから承認してください。

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動させ、ブラウザー キャンバスで更新されたユーザー インターフェイス（UI）を確認します。

> [!TIP]
> キャンバスは、Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを確認して作成しますが、ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトで、アプリを起動してブラウザー キャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価のあるゲーム カードに、5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプル リクエストを開いてマージする

機能ができました。新しいコードを既存のコードベースにマージするため、プル リクエストを作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR の作成前に該当する Issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューパネルで PR を開きます。必要に応じてここでレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- エージェント セッションを開始し、セッションの構成を確認しました。
- 小規模で対象を絞った変更をゲーム カードに加えるようエージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を開き、チェックをレビューして、明示的にマージしました。

次は、[フィルター機能の Issue から開始して Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
