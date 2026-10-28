---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の両方を一元的に扱うデスクトップアプリです。issue やプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、GitHub Copilot app を使い、Astro で構築された Tailspin Toys アプリをローカル環境で更新します。始める前に、ローカルに Node.js がインストールされていることを確認し、次に Copilot app をインストールしましょう。

このレッスンでは、次の操作を行います。

- プロジェクトのテストをコンピューターで実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を実装させ、Tailspin Toys のテストスイートをローカルで実行します。そのためには、プロジェクトに必要な唯一のランタイムである **[Node.js][nodejs]** が必要です。現在の **LTS** リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナルなど、普段使っているターミナルを開きます。
2. 次のコマンドを実行し、インストール済みの Node.js と npm のバージョンを確認します。

   ```shell
   node --version
   npm --version
   ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、以降の手順を実行してください。

4. [Node.js のダウンロードページ][node-download]を開きます。
5. 使用しているオペレーティングシステム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

   ```bash
   node --version
   npm --version
   ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストールや PATH の変更後は、アプリを完全に終了して再起動してください。セッション内でコマンドが引き続き失敗する場合は、[セッション環境のトラブルシューティング手順][session-environment]を確認してから続行します。

> [!IMPORTANT]
> E2E チェックには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree を準備する際は Tailspin Toys リポジトリの README に従い、インストールの要求を確認してから承認してください。

## ワークショップ用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレートリポジトリ][template-repository]から、今すぐ作成しましょう。新しいリポジトリにはラボに必要なファイルがすべて含まれます。次のレッスンでこのリポジトリをアプリに接続します。

1. 新しいブラウザーウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、次に **Create a new repository** を選択して自分用のコピーを作成します。

   ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft 主催イベントの一環としてワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に用意されます。ワークショップではこの issue を使うため、自分で登録する必要はありません。

ワークショップ用テンプレートの新しいコピーを使ってください。リポジトリには指示ファイル、アプリケーションコード、テスト、`quality-checks` スキル、既存の Canvas 拡張機能が含まれます。ワークショップ中にスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使用する場合は、必要なファイルが含まれていることを進行役に確認してください。

## まとめと次のステップ

準備が整いました。このレッスンでは、次の操作を行いました。

- プロジェクトをコンピューター上でビルドしてテストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートリポジトリからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
