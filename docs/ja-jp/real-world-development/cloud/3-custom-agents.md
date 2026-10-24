---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた人工知能（AI）アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に与えます。チームの知識を再利用できる形にまとめることで、[Web Content Accessibility Guidelines（WCAG）][wcag]への準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML front matter と、エージェントの振る舞い、専門分野、指示を定義する Markdown プロンプトを記載します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept]には一部共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一環として skill やほかのエージェントを調整できます。このラボでは、アクセシビリティのカスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業でプル リクエストのワークフローを行う skill やテストを実行、管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを行います。

- カスタム エージェントの定義方法を確認する。
- タスクをカスタム エージェントに割り当てる。

## シナリオ

Tailspin Toys は、視覚的な特性や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダーク テーマが読みにくいという声がありました。そこでデザイン チームは、ユーザーが切り替えられる高コントラスト モードの実装を依頼しました。WCAG への準拠を確認するカスタム エージェントも、チームの専門知識を再利用する例です。

アクセシビリティは重要なため、カスタム エージェントを使って必要な機能をすばやく作成します。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot に与えるガイダンスを把握します。

1. codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML front matter を確認します。

> [!CAUTION]
> `name` と `description` を含む front matter は、カスタム エージェントに必須です。

3. 続くセクションを読み、次の項目を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベスト プラクティス
   - HyperText Markup Language（HTML）、Cascading Style Sheets（CSS）、JavaScript のコード例
   - よくある落とし穴や間違い

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する場所です。ここから Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示を出せます。高コントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントとアクセシビリティ エージェントが強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは特定のタスクや分野に特化した AI アシスタントです。チームの知識と標準を再利用できる形にまとめ、Copilot が作業を効果的に進めるためのガイダンスを提供できます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- タスクをカスタム エージェントに割り当てました。

Copilot が高コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して誘導します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
