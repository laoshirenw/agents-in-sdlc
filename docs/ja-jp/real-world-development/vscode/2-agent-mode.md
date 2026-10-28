---
title: "エクササイズ 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題を修正し、見落としたファイルを探して再度変更する、という作業を繰り返します。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に作業します。既存のプロジェクト構成を調査し、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加してみましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。Agent mode は IDE 内で動作するため、すばやいフィードバックと対話が可能です。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

このエクササイズでは、次の操作を学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して、複数のファイルにまたがる変更を行う方法。
- UI とデータレイヤーにまたがる新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームが増えてきたため、利用者がカテゴリと出版社で絞り込めるようにします。前のエクササイズで出版社ヘルパーを追加しました。残りのデータレイヤー、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を確認します。Tailspin Toys は、開発者をテーマにしたボードゲームのクラウドファンディングサイトです。ゲームの一覧や詳細を表示する単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter から Drizzle ORM を通じてローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。データアクセス用の再利用可能なヘルパーは `src/lib/` にあり、操作性は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前のエクササイズを終えて `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace で新しいターミナルを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを開くと、次の機能を確認できます。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次のエクササイズを始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択はエクササイズごとに異なるため、必要な場合は手順内で説明します。

サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成したときに自動登録されたバックログ issue を Copilot に表示してもらいましょう。

1. Chat ビューの agent picker から **Agent** を選択します。**Agent** は複数ファイルにまたがる変更を自律的に計画・実装し、ターミナルコマンドとツールを実行します。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めることが目的ですが、LLM は確率的に動作するため、例とは異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使うことがありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを送信し、バックログについて Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は、利用者にとって有益だと考える順に issue を優先順位付けします。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。サイトの機能追加方法に関するガイダンスを含む [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネント設計、テスト容易性、アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンクが含まれます。

> [!TIP]
> 指示ファイルから他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用可能な小さなファイルに分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込みを完成するには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) の絞り込みロジックを追加または改善します。
- 絞り込み動作のテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

コードベースにマージする前に、テストも実行して成功させる必要があります。Copilot Agent Mode にこれらを依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** を選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent picker で **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** であることを確認します。
5. 関連するバックログ issue の要件に基づいて機能を実装するよう、次のプロンプトで依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot はプロジェクトを調査し、機能に関係するデータレイヤー、UI、テストのファイルを特定して変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は調査、ファイル変更、テスト実行などを行うため、タスクの複雑さやコードベースによって数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。すべての作業が終わると各変更に **Keep** または **Undo** が表示されるため、途中で選択する必要はありません。

7. Copilot が求めたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで作業します。

   ![Copilot Agent Mode の Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作とコードの確認が終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加しました。このエクササイズでは、次の操作を学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーをまたぐ新機能を実装する方法。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを見つけ、複数ファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスエクササイズ: ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。このエクササイズで学んだことを使い、Copilot にページネーションの実装を依頼してみましょう。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズを利用者が選択できるようにするか、固定にするかを検討します。
- 希望どおりの実装に必要な指示をプロンプトに含めます。
- Copilot に追加の変更を依頼してコンテキストを与えながら、何度か反復することもあります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
