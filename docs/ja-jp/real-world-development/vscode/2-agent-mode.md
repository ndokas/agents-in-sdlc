---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイルやテストの実行が必要な場合があります。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題をデバッグして、更新が漏れていたファイルを見つけ、再度変更します。Copilot Agent Mode は、こうした作業を支援します。

Copilot Agent Mode は統合開発環境（IDE）内でより自律的に作業します。既存のプロジェクト構造を調査し、必要な変更を行い、テストなどを実行して、見つかった問題を自動的に修正します。ここでは、Agent Mode でサイトに新機能を追加します。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる作業方法です。Agent mode は IDE 内で動作し、すばやくフィードバックを得ながら対話できます。Cloud agent は Issue とプル リクエストを通じてやり取りし、チーム メンバーのように非同期で作業します。

この演習では、次のことを行います。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して連携した変更を行う。
- ユーザー インターフェイス（UI）とデータ レイヤーをまたいで新機能を実装する。
- コードベースにマージする前に、生成された変更とテストをレビューする。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリと発行者で絞り込めるようにします。前の演習で publishers helper を追加しました。ここでは Copilot Agent Mode を使い、残りのデータ レイヤー、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を始める前に、Tailspin Toys Web サイトを開いて現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的な HyperText Markup Language（HTML）として生成します。ページの front matter から Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別のバックエンド アプリケーション プログラミング インターフェイス（API）やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス helper は `src/lib/` にあり、必要な対話機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace では、次の手順で実行できます。

1. 前の演習後に `main` にいることを確認し、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、Codespace 内で新しいターミナルを開きます。
3. `main` を更新して、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含む表示が出ます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くか確認するダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーの `http://localhost:4321` アドレスを選択します。

> [!NOTE]
> codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは codespace でホストされている Web サーバーへのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、現在の機能を確認します。

- **Home Page**: タイトル、画像、説明とともにボードゲーム一覧を表示します。
- **Game Details Page**: ゲームを選ぶと、タイトル、説明、発行者、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を開始します。モードやモデルは演習によって異なるため、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能で拡張します。テンプレートからプロジェクトを作成すると、GitHub Issue のバックログも自動作成されています。Copilot に項目を表示してもらいます。

1. Chat ビューのエージェント一覧で **Agent** を選びます。Agent はファイルをまたいで自律的に計画と実装を行い、ターミナル コマンドやツールを実行します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップで特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。出力の一貫性を高めることを意図していますが、大規模言語モデル (LLM) は確率的に動作するため、提案が例と異なる場合があります。これは正常な動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の Model Context Protocol（MCP）コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Issue 一覧を取得するコマンドの実行を許可するため **Continue** を選択します。
5. 生成された Issue 一覧を確認します。

Copilot はユーザーにとって有用だと判断した順に、項目を優先順位付けします。

## Instruction file を確認する

コード生成をエージェントに依頼する前に、Copilot にコンテキストを伝える instruction file を確認します。Web サイトへの機能追加方法を説明する [user interface (UI) instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のガイダンスを確認します。アーキテクチャの概要、コンポーネント設計・テスト容易性・アクセシビリティの原則、Astro・Tailwind CSS・Drizzle などファイル種別ごとの instruction file へのリンクが含まれています。

> [!TIP]
> Instruction file から、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。これにより命令を再利用可能な小さな単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) のフィルタリング ロジックを追加または改善する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

コードベースにマージする前に、テストを実行して成功することも必要です。Copilot Agent Mode に作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を追加します。

   ![UI instructions file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、データ レイヤー、UI、テストの関連ファイルを見つけ、変更とテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行います。コードベースの複雑さによって数分かかる場合があります。処理中にエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はなく、完了するとすべての変更に対して選択できるようになります。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 途中で一時停止したりテストに失敗したりする場合があります。Copilot は、エラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードも、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使い、Tailspin Toys Web サイトに新機能を追加しました。

- Agent Mode でプロジェクトを調査し、関連ファイルを特定して連携した変更を行いました。
- UI とデータ レイヤーにわたる機能を実装しました。
- コードベースにマージする前に、生成された変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が大きくなると、ページネーションが必要になります。この追加演習では、学んだ方法を使って Copilot に実装を依頼します。

1. 既存の instruction file などのベスト プラクティスに従い、Copilot にページネーションを実装するよう依頼します。
2. ページ サイズをユーザーに選ばせるか、固定するかを決めます。希望する実装に必要なガイダンスをプロンプトに含めます。
3. 必要に応じて追加の変更やコンテキストを依頼します。Copilot と反復して作業するのは通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
