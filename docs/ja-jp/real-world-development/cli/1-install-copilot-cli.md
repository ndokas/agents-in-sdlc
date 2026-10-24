---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、環境を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をコマンド ラインから行えます。タスクを任せたり、変更を依頼したりしながら作業に集中できます。まずはツールをインストールします。使い慣れたツールで簡単にセットアップできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼して、短い会話を試す。
- 組み込みの GitHub Model Context Protocol（MCP）サーバーを使ってフィルタリングの Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに取り組むために人工知能（AI）エージェントを使い始めています。多くの開発者が日常的に使うターミナルで、その機能を利用できるのが Copilot CLI です。このレッスンでは、インストールと認証を済ませ、ワークショップ全体で利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

初めて起動するとき、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次の確認が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま続行できます。

## 操作に慣れる

通常の shell プロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語がエージェントへの依頼となり、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みバージョンで使えるコマンドを確認し、<kbd>Esc</kbd> を押して画面を閉じます。
3. Copilot に簡単な質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答する前にリポジトリを調査することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトでフィルタリングの Issue を探し、その Uniform Resource Locator（URL）を取得します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開いて Issue を読みます。最初の変更を行った後に使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。worktree は自動作成されません。各変更の前に機能ブランチを作成します。

## ワークショップ用の簡易設定を使う

通常、Copilot CLI は許可済みの範囲外でツールを使う前に確認します。このワークショップでは Codespace 内で承認プロンプトを省略し、演習に集中できるよう、`--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` はすべてのツール、パス、URL へのアクセスを許可する（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）設定です。Codespace や VM のような隔離環境でのみ使用し、日常開発の既定値として alias に設定しないでください。詳細は[ツール使用の許可と拒否][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を付けて、後のレッスンで Issue とプル リクエストを扱うために使う GitHub MCP の読み取り / 書き込みツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動保存します。後で命令を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、短い会話を試しました。
- 組み込みの GitHub MCP サーバーを使ってフィルタリングの Issue を見つけました。

次は、[最初の変更に取り組み][next-lesson]、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
