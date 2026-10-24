---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加に関する 3 つのタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザイン チームが改善を進めるための関連ゲーム機能、高コントラスト モードとライト モードを含むアクセシビリティ機能です。コード変更を確認し、必要に応じて Copilot にフィードバックを送り、改善します。

### シナリオ

生成人工知能（AI）を使っても、ソフトウェア設計と開発・運用（DevOps）の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人の監視なしに作業するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コード変更に使うブランチに限り書き込みアクセス権を持ちます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人による承認が必要です。
- Model Context Protocol（MCP）サーバーを含め、外部リソースへのアクセスは[既定で制限されています][agent-firewall]。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初のプル リクエスト (PR) を確認し、コードにドキュメントを追加します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR で GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は既定で外部リソース（外部 MCP サーバーへの接続を含む）へのアクセスが制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Pull Requests** を選択します。
3. **Add missing documentation** など、ドキュメント追加に関する PR を開きます。タイトルは多少異なる場合があります。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。Copilot が完了するまで待ってください。数分かかる場合があります。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメント コメントなど、更新されたコードを確認します。変更内容は実行ごとに異なります。演習 2 の開始時に確認した instruction file に基づいて、TSDoc コメント、TypeScript の規約、コメント ヘッダーが追加されているかを確認してください。
6. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェック セクションで実行待ちになります。単一の Astro アプリに対するプロジェクトのチェックが成功することを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを受け取り、変更に対応する新しいセッションを開始します。結果は実行ごとに異なるため、特定の文面を指定することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を説明するコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. PR にコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションして、生成されたドキュメントへの変更を依頼します。上記の例や、コードベースに必要な別の変更を依頼してください。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しい commit が追加されます。
5. **Files changed** タブを選択して、変更をレビューします。

納得できるまで反復できます。完了したら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲーム機能の PR を開きます。タイトルは多少異なる場合があります。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションでワークフローが実行され、単一の Astro アプリに対するプロジェクトのチェックが成功することを確認します。完了まで数分かかる場合があります。
9. **任意:** Codespace でブランチを切り替え、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同じように、cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc コメントの追加を依頼したりできます。このタスクを割り当てた時点では、カスタム命令を更新する前だったことに注意してください。変更を依頼するには **Conversation** タブでコメントを追加します。Copilot がコメントを受け取り、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 で mission control から依頼したライト モードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、アクセシビリティ機能の PR を開きます。タイトルは多少異なる場合があります。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了まで数分待ってください。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目します。
   - 表示モードを切り替えるユーザー インターフェイス（UI）コンポーネント
   - ユーザー設定を保存する local storage の使用
   - 高コントラスト モードとライト モードの Cascading Style Sheets（CSS）またはスタイル変更
   - アクセシビリティ属性（Accessible Rich Internet Applications（ARIA）ラベル、キーボード ナビゲーションなど）
   - 表示モードを切り替える JavaScript / TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows ボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローが実行され、単一の Astro アプリに対するプロジェクトのチェックが成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチを切り替え、アクセシビリティ機能を手動でテストできます。Copilot が作成したブランチ名に置き換えて、ターミナルで次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーで高コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用のカスタム エージェントが、ベスト プラクティスに従った機能実装を Copilot に促したことを確認しました。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent には、実際のバックログ項目を任せて別の作業に集中できます。リポジトリで Issue をいくつか作成し、Copilot に割り当ててみましょう。たとえば次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent 環境の演習を完了しました。

- リポジトリに含まれるカスタム命令を確認し、cloud agent の成果物への影響を把握しました。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ向けのカスタム エージェントを作成、使用し、高コントラスト モードとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使用し、アクセシビリティ セッションを監視して誘導しました。
- Copilot が作成したプル リクエストをレビューし、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent 環境での演習は完了です。ほかの環境では、ここで試した内容を別の方法で実践できます。

- 🖥️ **[VS Code 環境](../../vscode/)** — 統合開発環境（IDE）で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI 環境](../../cli/)** — Plan モードと Autopilot モード、スキル、カスタム品質保証（QA）エージェント、Playwright MCP、Agent Merge、コンテキストを踏まえた引き継ぎを使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次の作業も試してください。

- リファクタリングやテスト カバレッジに関する Issue を cloud agent に割り当てる。
- ほかの分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
