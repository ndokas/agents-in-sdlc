---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys 用の Node.js をインストールし、テンプレートからリポジトリのコピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を一元的に扱うデスクトップ アプリです。Issue やプル リクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、ローカルで Tailspin Toys アプリを更新します。アプリは Astro で構築されています。始める前に Node.js がローカルにインストールされていることを確認し、次に Copilot app をインストールします。

このレッスンでは、次のことを行います。

- プロジェクトのテストをコンピューターで実行するために Node.js をインストールする。
- テンプレートから Tailspin Toys プロジェクトのコピーを作成する。

## Node.js をインストールする

エージェントに機能を作成させ、Tailspin Toys のテスト スイートをローカルで実行するレッスンがあります。そのためには **[Node.js][nodejs]** が必要です。プロジェクトに必要なランタイムは Node.js のみです。現在の長期サポート（LTS）リリースをインストールしてください。

すべてのプラットフォームで、公式インストーラーを使う方法が最も簡単です。

1. オペレーティング システムで、Windows Terminal、macOS のターミナルなど、普段使っているターミナルを開きます。
2. 次のコマンドを実行し、インストール済みの Node.js と npm のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. 両方のコマンドが成功し、Node.js のバージョンがプロジェクトの README と `package.json` に記載された要件を満たしていれば、次のセクションに進みます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、次のインストール手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストール済みの Node.js と npm のバージョンが表示されることを確認します。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリで使えるとは限りません。Node.js をインストールした後、または PATH を変更した後は、アプリを完全に終了して再起動してください。セッション内でどちらかのコマンドが引き続き失敗する場合は、続行する前に[セッション環境のトラブルシューティング][session-environment]を行います。

> [!IMPORTANT]
> 各 worktree でエンドツーエンド（E2E）チェックを行うには、プロジェクトの依存関係と Playwright Chromium も必要です。worktree の準備については Tailspin Toys リポジトリの README に従い、インストール要求を承認する前に内容を確認してください。

## ラボ リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][template-repository]から作成してください。新しいリポジトリにはラボに必要なファイルが含まれます。次のレッスンでアプリに接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ リポジトリのページで **Use this template** を選択し、続けて **Create a new repository** を選択します。

    ![Use this template ボタンのドロップダウンで Create a new repository が選択されている画面](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成できます。

    ![github-samples/tailspin-toys をテンプレートに指定し、リポジトリ名を入力した Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。後のレッスンで使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップ全体でこれらの Issue を使うため、自分で登録する必要はありません。

新しいワークショップ テンプレートのコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、`quality-checks` skill、既存のキャンバス拡張機能が含まれます。ワークショップでは、この skill をカスタマイズして品質保証（QA）エージェントを作成します。古いコピーを使う場合は、必要なファイルが含まれているか講師に確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- プロジェクトを自分のコンピューターでビルド、テストするために Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリのコピーを作成しました。

次は、[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続してワークスペースを確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
