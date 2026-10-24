---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた人工知能（AI）アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、指示、ベスト プラクティス、コーディング パターン、専門知識を与えます。チームの知識を再利用できる形にまとめることで、[Web Content Accessibility Guidelines（WCAG）][wcag]への準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML front matter と、エージェントの振る舞い、専門知識、命令を定義する Markdown プロンプトを記載します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept]には一部共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、skill やほかのエージェントを調整できます。このラボでは、アクセシビリティのカスタム エージェントがガイドラインに従ってサイトをレビュー、更新します。その作業でプル リクエストのワークフローを行う skill や、テストを実行、管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、文字と背景色のコントラストが不十分で、現在のダーク テーマが読みにくいという声がありました。デザイン チームは、ユーザーが切り替えられる高コントラスト モードの実装を依頼しました。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装する。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot に与えるガイダンスを把握します。演習 3 のフィルタリング PR とは分けて作業するため、まず `main` から新しいブランチを作成します。

1. codespace に戻り、ターミナルを開いて次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML front matter を確認します。

> [!CAUTION]
> `name` と `description` を含む front matter は、カスタム エージェントに必須です。

4. 続くセクションを読み、次の項目を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベスト プラクティス
   - HyperText Markup Language（HTML）、Cascading Style Sheets（CSS）、JavaScript のコード例
   - よくある落とし穴や間違い

## Copilot Chat でカスタム エージェントを使う

VS Code では `.github/agents` に定義されたカスタム エージェントが、Copilot Chat ビュー下部のエージェント一覧に表示されます。カスタム エージェントを選ぶと、そのエージェントの命令とツールを使うようチャットを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の操作を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント一覧で、まだ選択されていない場合は **Agent** を選びます。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント一覧を選択します。現在のエージェント名が表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに、次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルに適用します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. エージェントが変更した作業ファイルの一覧が **Files changed** に反映されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなど、リポジトリ内のファイルを実際に変更します。

次の演習では、この作業を監視し、途中で追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。特定のタスクや分野に特化したエージェントを使うと、チームの知識と標準を再利用できる形にまとめ、Copilot の作業を導けます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- 既存のアクセシビリティ用カスタム エージェントを確認し、Copilot Chat で高コントラスト モードを実装するために使いました。

次は、[エージェントの作業を監視して誘導します][next-lesson]。変更を確認しながら、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [統合開発環境（IDE）でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
