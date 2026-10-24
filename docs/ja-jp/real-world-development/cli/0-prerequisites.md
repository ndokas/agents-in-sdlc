---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリのコピーを作成し、[codespace][codespaces] を起動します。次のレッスンで、統合ターミナルから Copilot CLI をインストールして実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトのコピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ラボ リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][tailspin-template]から作成します。新しいリポジトリにはラボに必要なファイルが含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. **Use this template**、続けて **Create a new repository** を選択してコピーを作成します。
3. GitHub または Microsoft が主催するイベントで受講している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えます。以降の演習で使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップでこれらの Issue を使うため、自分で作成する必要はありません。

新しいワークショップ テンプレートのコピーを使ってください。リポジトリには命令、アプリケーション コード、テスト、`quality-checks` skill、演習で使うバックログが含まれます。古いコピーを使う場合は、必要なファイルがあるかどうかを講師に確認してください。

## Codespace を作成する

次に、Codespace でワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した機能豊富なエディターを利用できます。

1. 新しく作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces がポート `4321` を利用可能と通知したら、**Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、テンプレートから Tailspin Toys のコピーを作成し、Codespace を作成してプロジェクトを確認しました。

次は Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
