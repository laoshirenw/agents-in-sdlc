---
title: "エクササイズ 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

[GitHub Copilot のカスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept]には、Markdown ファイルで定義して AI に作業方法を伝えるという共通点があります。簡潔に言えば、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。このワークショップでは、アクセシビリティエージェントがサイトをアクセシビリティガイドラインに照らして確認・更新します。その作業の中で、プルリクエストワークフローのスキルや、テストを実行・管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の利用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングサイトを使えるようにしたいと考えています。最近のフィードバックによると、現在のダークテーマはテキストと背景色のコントラストが十分でなく、読みにくいと感じる利用者がいます。このアクセシビリティ上の課題に対処するため、デザインチームは利用者が切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なので、カスタムエージェントを使って機能をすばやく生成します。

このエクササイズでは、次の操作を行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに用意されています。指示内容を理解するため、Codespace に戻り、次のブランチに切り替えてからプロファイルを確認します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. カスタムエージェントに必要な YAML frontmatter の `name` と `description` を確認します。

> [!CAUTION]
> カスタムエージェントの frontmatter には `name` と `description` が必要です。

3. 続くセクションを読み、アクセシブルなサイト向けのコードを生成する際の主な責務、ベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にある agent picker には、`.github/agents` に定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、その指示とツールに基づいた会話になります。

> [!TIP]
> **Copilot Chat を開く**
>
> 次のエクササイズを始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択はエクササイズごとに異なるため、必要な場合は手順内で説明します。

1. Chat ビューの agent picker で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にある agent picker を選択します。現在のエージェントが表示されます。既定のエージェントは **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクトファイルに適用します。変更ごとにファイルパスと差分がチャットに表示されます。
6. エージェントが変更した作業セットに応じて、**Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は実際にリポジトリを変更し、Astro コンポーネント、CSS、関連テストなどを更新します。

次のエクササイズでは、作業中の変更を確認し、エージェントを導きます。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に特化した GitHub Copilot です。チームの専門知識と標準を再利用可能なエージェントにまとめ、特定の作業をより効果的に進められます。

このレッスンでは、次の内容を学びました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は[エージェントの作業を監視して導き][next-lesson]、変更を確認し、同じセッションでライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
