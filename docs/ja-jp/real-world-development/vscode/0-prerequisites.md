---
title: "エクササイズ 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code のエクササイズを始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ワークショップ用リポジトリをセットアップする

作業用のリポジトリを作るには、[テンプレート][template-repository]からインスタンスを作成します。新しいインスタンスにはワークショップに必要なファイルが含まれ、エクササイズを通じて使用できます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. リポジトリページの **Use this template** ボタンを選択し、**Create a new repository** を選択して自分用のコピーを作ります。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後のエクササイズで参照します。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップではこの issue を使います。自分で issue を登録する必要はありません。

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

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使用している場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

インストール後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、Codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して Enter キーを押します。Copilot Chat が有効になります。
4. 未認証の場合は、GitHub アカウントでのサインインを求められます。表示される手順に従って認証します。

   ![Copilot Chat の認証を求める画面の例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ワークショップ用リポジトリのコピーを作成し、コードを書くときに使用する Codespace の作成を開始できました。

## 次のステップ

Copilot を使って作業を始めましょう。[エクササイズ 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートリポジトリからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
