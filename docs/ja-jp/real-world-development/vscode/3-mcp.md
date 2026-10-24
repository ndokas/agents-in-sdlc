---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent mode でフィルタリング機能を作成しました。プル リクエスト（PR）を開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright Model Context Protocol（MCP）サーバー**を接続し、Copilot に実際のブラウザーを操作して機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- MCP と、MCP サーバーが Copilot にツールを追加する方法を学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に Playwright を使わせ、ブラウザーでフィルタリング機能を手動テストする。
- ブランチを公開し、フィルタリング作業のプル リクエストを開く。

## MCP とは

Agent mode はエディターの外部にもアクセスできると、さらに便利になります。Copilot が外部ツールやサービスと通信する標準の仕組みが MCP です。

![コンテキスト、大規模言語モデル（LLM）、ツールとのやり取り、および MCP サーバーと VS Code 拡張機能が提供するツールを含む agent mode の図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)を使うと、人工知能（AI）エージェントが外部ツールやサービスと通信できます。MCP は、AI エージェントが外部ツールやサービスとリアルタイムにやり取りする方法を提供します。リソースを通じて最新情報にアクセスし、ツールを使って開発者に代わる操作を実行できます。

ツールとリソースへのアクセスには MCP サーバーを使います。MCP サーバーは AI エージェントと外部ツールやサービスの間を仲介します。既存の API や NPM パッケージなどのローカル ツールとの通信を管理し、サーバーごとに異なるツールとリソースを提供します。

よく使われる MCP サーバーの例を示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するアプリケーション プログラミング インターフェイス（API）にアクセスできます。リポジトリの作成や更新、Issue とプル リクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースにアクセスする MCP サーバーがあります。GitHub は、エコシステム内の MCP を見つけやすくし、貢献を促進するための [MCP registry](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティ上、MCP サーバーはプロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに使えます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、画面を表示せずにブラウザーを実行します。デスクトップ画面のない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストにも Playwright を使っているため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択し、サーバーを起動して構成を確認します。
2. サーバーを信頼するか尋ねられた場合は、構成を確認して信頼することを選びます。

手動で起動する手順は任意です。省略した場合、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できる実際のブラウザーを提供します。開発者自身がアプリを操作して確認する代わりに、エージェントにページを開き、移動し、フィルターを適用して結果を読み上げてもらい、確認内容を要約させることができます。会話を離れずに機能の動作を確認する方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使って動作します。エージェントはボタン、リンク、リスト項目など、ラベル付けされた構造化要素を調べます。これは支援技術の動作と同様であり、機能の確認とあわせてアクセシビリティを簡単に確認できます。

サーバーを接続し、アプリを起動した状態で、Copilot に次のプロンプトを送ってフィルタリング機能を操作、検証してもらいます。

1. 次のプロンプトを送信します。

    ```text
    Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

    1. Go to the games page and note how many games are listed.
    2. Apply a category filter and confirm the list updates to only show games in that category.
    3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
    4. Combine a category and a publisher filter and confirm the results respect both.

    Report what you observe at each step, and call out anything that does not behave as expected.
    ```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ条件と照らし合わせます。問題があれば追加で質問するか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストでは、アプリを `http://localhost:4321` で起動しておく必要があります。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要な場合があります。

2. 開発サーバーが停止している場合は再起動してからプロンプトを再送信します。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してからテストを再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプル リクエスト (PR) を開きます。最初に `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを選択し、commit メッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プル リクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、PR の作成方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。Copilot は関連する Issue を見つけ、PR に関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを利用できるよう、エージェント一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集と操作を許可します。GitHub ツールを初めて使うとき、GitHub 認証を求められる場合があります。表示される手順に従って認証します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- MCP と、MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に Playwright でブラウザー上のフィルタリング機能をテストしました。
- ブランチを公開し、フィルタリング作業のプル リクエストを開きました。

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
