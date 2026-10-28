---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットも試したので、次は**エージェントセッション**を開始して、プロジェクトに最初の変更を加えましょう。変更は小さく保ちます。ゲームデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェントセッションを開始し、セッションの構成を学びます。
- エージェントに小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明しか表示されません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションに適した、小さく独立した変更です。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには**専用の Git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを実行できます。セッションはサイドバーにリポジトリごとにまとめられ、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調査・編集する際の**ツール操作**、差分付きの**変更ファイル一覧**が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを調べて機能を実装します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して、変更を依頼しましょう。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して、Copilot にプロンプトを送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成し、作業を開始します。次にプロジェクトを調べ、新機能の追加に必要なファイルを見つけてコードを作成します。Copilot app で新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更ファイルと差分を確認します。

   ![Create PR ボタンの右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームカードを表示する中心的なファイル `GameCard.astro` にコードが追加されているはずです。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" に切り替える、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、比較的よく似た実装になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と、`null` の場合の代替表示がテストされていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動くと判断せず、Copilot にサイトを起動させ、更新された UI を確認しましょう。サイトを起動してブラウザーキャンバスで開くよう依頼します。

> [!TIP]
> キャンバスは Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンでカスタムキャンバスを調べ、自分でも作成します。ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスでページを開くよう依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに5点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを開いてマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするプルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の案内に従って認証します。
3. リポジトリのポリシーにより、PR の作成前に関連する issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択してレビューペインで PR を開き、確認します。必要に応じてここでレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択して、プルリクエストをマージします。

## まとめと次のステップ

GitHub Copilot app で最初の変更をリリースしました。具体的には、次の操作を行いました。

- エージェントセッションを開始し、セッションの構成を学びました。
- ゲームカードに小さく対象を絞った変更を加えるよう、エージェントに指示しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を開き、チェックをレビューし、明示的にマージしました。

次は[絞り込み issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を実装します。

## リソース

- [GitHub Copilot app でエージェントセッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
