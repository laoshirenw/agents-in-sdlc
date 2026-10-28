---
title: "エクササイズ 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent のエクササイズを始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーと [Codespace][codespaces] を作成します。Codespace では指示ファイルを編集し、cloud agent が生成した作業をレビューできます。

## ワークショップ用リポジトリをセットアップする

作業用のリポジトリを作るには、[テンプレート][template-repository]からインスタンスを作成します。新しいインスタンスにはワークショップに必要なファイルが含まれ、エクササイズを通じて使用できます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. リポジトリページの **Use this template** ボタンを選択し、**Create a new repository** を選択して自分用のコピーを作ります。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後のエクササイズで参照します。

## Codespace を作成する

次に、Codespace を使ってエクササイズに取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザー上で直接コードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した高機能 IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスをすべて手動でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 後のエクササイズで Codespace に戻ります。ブラウザーのタブを閉じずに残しておいてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作するように構成されています。どちらも必要な前提条件を満たす環境です。ローカルで作業する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ワークショップ用リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業するときに使う Codespace の作成を開始できました。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[エクササイズ 1 - カスタム指示][next-lesson]に進みましょう。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートリポジトリからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
