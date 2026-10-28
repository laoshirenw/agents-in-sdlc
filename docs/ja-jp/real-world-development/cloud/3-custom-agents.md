---
title: "エクササイズ 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

[GitHub Copilot のカスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、そのエージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept]には、Markdown ファイルで定義して AI に作業方法を伝えるという共通点があります。簡潔に言えば、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。このラボでは、アクセシビリティカスタムエージェントがサイトをアクセシビリティガイドラインに照らして確認・更新します。その作業の中で、プルリクエストワークフローのスキルや、テストを実行・管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の利用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントについて、次の操作を学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスク割り当て。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングサイトを使えるようにしたいと考えています。最近のフィードバックによると、現在のダークテーマはテキストと背景色のコントラストが十分でなく、読みにくいと感じる利用者がいます。このアクセシビリティ上の課題に対処するため、デザインチームは利用者が切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なので、カスタムエージェントを使って機能をすばやく生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに用意されています。内容を確認して、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタムエージェントに必須です。

3. 続くセクションを読み、アクセシブルなサイト向けのコードを生成する際の主な責務、ベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## issue を作成して割り当てる

Mission control は、この環境のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を送れます。まず、高コントラストモードの作成を Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントと Accessibility が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に特化した GitHub Copilot です。チームの専門知識や標準を再利用可能なエージェントにまとめ、特定の作業をより効果的に進められます。

このレッスンでは、次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスク割り当て。

Copilot は高コントラストモードを実装しています。次は Mission control から[エージェントセッションを監視して導きます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
