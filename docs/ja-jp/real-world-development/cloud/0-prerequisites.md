---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、instruction file の編集や cloud agent の成果物のレビューに使う [codespace][codespaces] を起動します。

## ラボ リポジトリをセットアップする

作業用リポジトリのコピーを作成するには、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいリポジトリにはラボに必要なファイルが含まれ、演習中に使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** を選択し、続けて **Create a new repository** を選択してコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。以降の演習で使用します。

## codespace を作成する

次に、ラボの演習で使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した、機能豊富な統合開発環境（IDE）を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手作業でインストールするよりも短時間で済みます。待っている間に GitHub Copilot のほかの機能を確認できます。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ここでは、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも演習に必要な前提条件を備えています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ リポジトリのコピーを作成し、Copilot cloud agent と作業するための codespace の作成を開始しました。

## 次のステップ

次に、cloud agent が従うカスタム命令を追加します。[演習 1 - カスタム命令][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
