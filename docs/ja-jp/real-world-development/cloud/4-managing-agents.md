---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー体験の改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。この mission control では、オープンなプル リクエストがあるエージェント タスクを確認できます。実行内容を調べ、進行中のセッションに追加指示を出して作業を誘導することもできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視する。
- 実行中のセッションを誘導し、機能追加を依頼する。

## シナリオ

高コントラスト モードを依頼した後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能を更新する作業が進んでいるため、ライト モードも含めるのが合理的です。高コントラストに加えてライト モードも追加するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左側のペインに表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメント更新
   - ゲームを変更するアプリケーション プログラミング インターフェイス（API）の生成
   - Web サイトへの高コントラスト モードの追加
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。コードのチェックアウト、作業環境の作成、Model Context Protocol（MCP）サーバーのセットアップ、タスクを完了するための各種作業などが含まれる場合があります。

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト（PR）ペインも確認します。ここから PR と変更ファイルを確認し、作業を監視できます。

## cloud agent を誘導する

進行中のタスクを確認したので、高コントラスト モードの作業中にライト モードの切り替えも追加するよう依頼します。

1. 高コントラスト モードを追加しているセッションを選択します。表示名は Copilot が付けた名前や作業状況により異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分待ちます。作業開始後は、以下の画面例に似たメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページで作業中の cloud agent タスクと、Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業計画に追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順を終えた後、新しい依頼を作業に取り込みます。

これまでと同じく、完了まで数分かかります。その間に、ここまでに学んだことを振り返ってください。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、誘導するための中心的な場所である Copilot agents ページを確認しました。mission control では、進行中と完了済みのタスクを追跡し、実行中の作業を確認して、追加の指示で作業範囲を調整できます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプル リクエストを反復的に改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
