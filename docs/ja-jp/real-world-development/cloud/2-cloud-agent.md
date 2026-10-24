---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が完了すると、人がレビューできるドラフト プル リクエスト（PR）を作成します。手間のかかる作業を任せることで開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する方法を確認する。
- Issue の範囲を明確にする重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、改善したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを残しておらず、まずアプリケーション内のすべてのエクスポート関数に TypeScript 用ドキュメント コメント（TSDoc）を追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリのゲームを表示し、閲覧を続けられるようにします。現時点では完成度の高い実装ではなく、ユーザー体験（UX）の受け入れテストに使えるものを求めています。この作業は重要ですが、より優先度の高い Issue もあります。

このように後回しになりやすい作業は、Copilot cloud agent に適しています。人工知能（AI）のペア プログラマーとして非同期で作業を任せ、開発者は別のタスクに集中した後で成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当て後、Copilot は進捗を追跡するドラフト PR を作成し、環境をセットアップして作業を開始します。実行中または完了後にセッションを確認できます。解決案をレビューできる状態になると、PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見単純な操作にも、詳細を確認すると複雑さがあります。[Copilot cloud agent に依頼する作業の範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして段階的に作業し、学び、試し、調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令が Copilot にどのようなガイダンスを与えるかを確認しました。すでにカスタム命令の演習を完了している場合は、`.github/copilot-instructions.md` が機能する様子を見ています。まだの場合は、この機会に確認してください。Issue を Copilot cloud agent に割り当てる前に、既存の instruction file を読み取り専用で確認し、後からその影響を見られるようにします。

リポジトリの GitHub Web ユーザー インターフェイス（UI）または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript 規約と TSDoc ドキュメント コメントに関する要件を確認してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` front matter を確認します。この命令は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript 規約、コメント ヘッダーが反映されているかを確認します。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するには、通常、必要な環境を用意し、初期状態を整えるためのセットアップ スクリプトを実行します。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアと同じように作業します。

Cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業前に実行する開発ツールや依存関係を設定できます。ラボの流れを円滑にし、学習機会を提供するため、この設定はあらかじめ用意されています。Node.js、プロジェクトの依存関係、エンドツーエンド（E2E）テスト用のブラウザー、マイグレーションとシードを済ませた単一 Astro アプリ用の SQLite データベースが使用可能になります。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` が必要です。クローンしない場合、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（現在、アプリ全体は Astro と Drizzle/libSQL で構成されています）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様の形式ですが、いくつかの点に注目してください。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、ワークフローが成功するかテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古かったり、不足していたりします。この技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を支援できます。Copilot cloud agent に Issue を割り当て、バックグラウンドで必要なドキュメントを生成してもらいます。後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に Copilot が作業リストを作成したことを確認します。

> [!NOTE]
> Copilot が作業リストを PR に表示するまで数分かかる場合があります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、タスクに取り組む方法を決定しています。

13. 作業リストと実行予定のタスクを確認します。
14. プル リクエストのタイムラインを下へスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 状態の表示を更新するため、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する過程を確認します。コードの調査、現在の状態の把握、計画を決めるための検討、コード作成などが行われます。

完了まで数分かかる場合があります。Copilot cloud agent の主な利点は、非同期で作業を任せ、開発者が別の作業に集中できることです。この利点を活用するため、もう 1 つのタスクを cloud agent に割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent を使うと作業を分担でき、開発者は別のタスクに集中できます。デザイン チームが希望する関連ゲームの追加にかかる時間が短くても、その時間を別の作業に使えます。cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組みます。cloud agent はソフトウェア エンジニアのように作業するため、完了まで常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]に Issue を割り当て、非同期で作業を依頼できることを確認しました。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- Issue の範囲を明確にする重要性を理解しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は Model Context Protocol（MCP）サーバーも使用でき][cloud-agent-mcp]、前の演習で確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
