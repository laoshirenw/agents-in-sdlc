---
title: "エクササイズ 4 - エージェントを監視・管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つのエクササイズでは、Copilot cloud agent にユーザー体験の改善と機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションを導いて作業を支援したりすることもできます。

このレッスンでは、次の操作を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションを導いて機能の追加を依頼します。

## シナリオ

高コントラストモードを実装するようエージェントに依頼した後、チームはライトモードも追加する良い機会だと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、同じ作業に含めるのが合理的です。高コントラストだけでなくライトモードも追加するよう、エージェントを導きます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。リポジトリのチェックアウト、Copilot 用環境の作成、MCP サーバーの設定、タスクを完了するための各種作業などが表示されます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更ファイルを確認して、作業を監視できます。

## cloud agent を導く

実行中のタスクを確認したので、高コントラストモードに取り組んでいる Copilot に、ライトモードの切り替えも追加するよう依頼します。

1. 高コントラストモードを追加するセッションを選択します。エージェントが設定した名前や作業状態によって、セッションの正確なタイトルは異なります。

   ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents ページでアクティブセッションへの追加指示ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に加えたことを確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取りかかります。プロンプトを送信した時点で実行中のステップが終わると、新しい要望を作業の流れに反映します。

前回と同様、完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、cloud agent のタスクを監視して導く方法を学びました。この mission control では、進行中と完了済みのタスクを追跡し、実行された作業を確認し、進行中のタスクに追加の指示を送れます。

このレッスンでは、次の操作を行いました。

- mission control とエージェントページを使って cloud agent のタスクを監視しました。
- 進行中のセッションに機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装する間に、次は作成されたプルリクエストの[反復改善][next-lesson]に進みます。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
