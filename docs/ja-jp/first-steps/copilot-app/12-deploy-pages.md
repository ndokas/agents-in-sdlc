---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app で Pages ワークフローを構成し、プル リクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開し、誰でもアクセスできる場所を用意します。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages の構成を依頼する。
- マージ前にワークフローとプル リクエストをレビューする。
- デプロイを確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイムの依存関係を追加せずに、公開された Uniform Resource Locator（URL）からアクセスできるようになります。

## Copilot にサイトの公開を依頼する

`space-quiz` の Copilot app セッションで、GitHub Pages を構成するよう次のプロンプトを送信します。

1. 次のプロンプトを送信します。

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![マージ済みの space-quiz プル リクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、および yourname.github.io/space-quiz/ に公開された Space Quiz の 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、プル リクエストの作成とマージ、およびリポジトリの Pages 設定の変更をエージェントに依頼します。差分、デプロイ権限、GitHub の承認要求を確認してから進めてください。

## ワークフローをレビューしてデプロイを確認する

変更をマージする前に、ワークフローとアクセス許可を確認します。その後、デプロイを追跡し、公開されたサイトにアクセスできることを確かめます。

1. リポジトリに README がない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順を含め、プル リクエストの変更をレビューします。
3. 必須のプル リクエスト チェックが成功した後にのみ、マージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

6. エージェントが Pages を有効にできない場合やプル リクエストをマージできない場合は、提示された具体的な手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで、作業を完了とみなさないでください。

## まとめと次のステップ

Space Quiz を公開サイトとして利用できるようにし、エージェントの報告だけに頼らずデプロイを確認しました。このレッスンでは、次のことを行いました。

- Copilot に GitHub Pages の構成を依頼しました。
- マージ前にワークフローとプル リクエストをレビューしました。
- デプロイを確認し、公開されたサイトを検証しました。

[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
