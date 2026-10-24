---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続してワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、継続的インテグレーション（CI）パイプラインをすぐに利用できます。自分だけですべての作業を行うのではなく、複数のエージェントをそれぞれ分離されたワークスペースで並列に指示し、繰り返しのタスクを自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトの準備ができたので、アプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認する。
- クイックチャットでアプリについて質問する。

## シナリオ

チームは、増え続けるバックログに対応するために人工知能（AI）エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プル リクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについて会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン（Pro、Pro+、Business、Enterprise）を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用するプラットフォーム向けのアプリをダウンロードし、ランディング ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ レジデンシーを使用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリの選択を求められた場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されているか確認します。すでに表示されている場合、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横の **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとめられ、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには、次の領域があります。

- **New** - Copilot との新しいチャット セッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じて、プル リクエストを表示します。アプリを離れずに、一覧表示や絞り込み、CI ステータスの確認、レビューを行えます。
- **Issues** - Issue を表示します。一覧表示や絞り込み、Issue からのセッション開始ができます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。ToDo リストの管理や定期的なプロジェクト保守などの作業に役立ちます。まとめでは次のステップとして紹介しますが、ワークショップの演習では扱いません。
- **Customize** - Model Context Protocol（MCP）サーバー、プラグイン、スキルなどを追加して Copilot app の機能を拡張します。Playwright MCP の構成に使用します。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの軽い会話です。最初のチャットを始めると表示されます。まだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。
- **Sessions** - リポジトリ フォルダーとエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリ ビューが開き、その下のセッションを選ぶと会話に戻ります。各セッションは分離された独自のワークスペースで実行されるため、変更を競合させずに複数のセッションを同時に実行できます。星評価を追加する次のレッスンで、最初のセッションを開始します。

ワークショップを進めながら、各領域を使っていきます。

> [!TIP]
> 操作方法がわからない場合や、できることを知りたい場合は、Copilot に質問できます。

### 用意されたバックログを確認する

ほとんどのプロジェクトにバックログがあり、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号ではなく、次のタイトルで Issue を探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を確認します。各 Issue からエージェント セッションを開始できます。最初の小さな変更を完了した後、フィルター機能の Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は、Copilot app に追加したリポジトリの項目だけを表示するよう自動的に絞り込まれます。ほかのリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問してみましょう。**クイックチャット**なら、ブランチや worktree を作らずに質問やブレインストーミングができ、セッションを必要としない簡単な質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションの仕組みについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の Git worktree で実行されるため、変更を競合させずに複数のエージェントを並列実行できることがわかります。会話は続けられ、いつでも新しいチャットを開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。このレッスンでは、次のことを行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで、その場で質問しました。

次は、[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
