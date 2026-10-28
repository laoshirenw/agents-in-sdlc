---
title: "エクササイズ 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に行います。作業が完了すると、人間によるレビュー用のドラフト PR を作成します。面倒な作業を任せることで、開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次の操作を学びます。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 明確な範囲設定がされた issue の重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。初期サイトの作成を担当した委託先がドキュメントを適切に整備せず、現在のコードにはドキュメントがありません。まず、アプリケーションのエクスポート関数すべてに TSDoc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示します。完成度の高い実装はまだ必要なく、UX の受け入れテストに使えるものを求めています。現在は後回しになっていますが、ほかの issue の優先度がより高い状態です。

どちらも優先順位が下がりやすいタスクであり、Copilot cloud agent に任せるのに適しています。非同期で作業させて別のタスクに集中し、後から戻って作業をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば [GitHub issue を Copilot に割り当て][assign-issue]る方法があります。割り当てられると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに取り組みます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、Copilot はプルリクエストで通知します。

## 適切な範囲設定の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑さがあります。[Copilot cloud agent のタスク範囲を慎重に設定][cloud-agent-best-practices]してください。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前のエクササイズでは、カスタム指示と Copilot への影響を確認しました。すでに指示ファイルのエクササイズを終えている場合は `.github/copilot-instructions.md` を実際に使いました。まだの場合は、cloud agent に作業を割り当てる前に、リポジトリに用意された指示を読み取り専用で確認して、後で成果にどう表れるかを見てみましょう。

GitHub Web UI または起動済みの Codespace で、次のファイルを確認します。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` に指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成されたプルリクエストに TSDoc コメント、TypeScript 規約、コメントヘッダーが反映されます。後のエクササイズでプルリクエストをレビューする際に、もう一度確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は開発環境の準備とセットアップスクリプトの実行が必要です。SWE のように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行するツールや依存関係を指定できます。このラボの流れを円滑にして学習機会を提供するため、この設定は事前に用意されています。Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # この手順に必要な最低限の権限を設定します。Copilot には操作用の独自トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / app のセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに正常動作を確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なので、この issue を cloud agent に割り当ててみましょう。エージェントはバックグラウンドで必要なドキュメントを作成します。後のエクササイズで結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された、`[WIP]: Code lacks documentation` のようなタイトルのプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な対処方法を検討しています。

13. todo リストを読み、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新しないと、ステータスが反映されない場合があります。

16. Copilot の進行中のセッションをスクロールして確認します。コードを調査して状態を把握し、計画を考えて、コードを作成するまでの動作を観察できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、非同期にタスクを行い、別の作業に集中できるようにすることです。その機能を使い、もう1つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業を始めます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の1つは作業を分けられることです。デザインチームが求める関連ゲーム機能は長時間かからないかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当ててみましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が issue の作業中を示すために目の絵文字を使う画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。SWE と同様に作業する cloud agent を常時監視する必要はありません。完了したら結果をレビューします。その間にカスタムエージェントを作成して使いましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って issue を割り当て、非同期で作業させる方法を学びました。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

このレッスンでは、次の内容を学びました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 明確な範囲設定がされた issue の重要性。
- issue を Copilot に割り当てる方法。

Cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用を確認します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
