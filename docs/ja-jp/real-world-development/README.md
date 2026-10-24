---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。反復的に変更を計画してコードを作成し、動作をテストしてプル リクエストをレビューし、エージェントを使って作業を管理します。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として作業します。同社は開発者をテーマにしたボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub Issue として登録済みで、すぐに取り組めます。フィルタリングやページネーションなどの機能に加え、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、開発者が作業する環境に合わせて利用できます。希望する開発方法に合った環境を選んでください。各ワークショップはそれぞれセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright Model Context Protocol（MCP）、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

エージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、デスクトップ アプリから GitHub Issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を導き、進捗を監視し、エージェントが作成したプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
