---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは、絞り込み機能と品質ワークフロー全体を1つのセッションとブランチで進め、1つの PR にまとめています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を学びます。
- 機能全体の PR と検証の証拠を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

絞り込みの作業を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys では、マージの承認権限は開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app でプルリクエストを取り込むまでに必要な残りの作業を自動化します。有効にすると、app のセッションがプルリクエストを読み込み、CI チェックの失敗やレビューコメントなどの妨げを解決し、必要に応じて rebase を行います。GitHub が許可すると、変更をマージします。バックグラウンドで実行され、app を再起動しても継続し、プルリクエストのマージ後に自動的に停止します。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできますが、コードの編集やマージには明示的な承認が必要です。マージを許可する前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したら、Agent Merge に PR プロセスを管理させましょう。

1. エージェントピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンから項目を選ぶだけでは開始されません。

Agent Merge が開始され、次の作業を行います。

- タイトルと説明を含むプルリクエストを作成します。
- issue からセッションを開始した場合は、説明本文で関連 issue に言及します。
- ターゲットブランチに対して rebase を行うか、マージ競合に対処します。
- CI を監視して、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックがないか PR を監視し、コメントを解決するよう更新します。
- 任意で、すべて成功した後に PR を自動的にマージします。

すべてのチェックが成功したら、Agent Merge に PR をマージさせましょう。

5. マージを許可する前に、PR の差分と検証の証拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントにマージを明示的に依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 処理を確認し、報告されたブロッカーを解消します。続行前に PR の状態が **Merged** になっていることを確認します。マージ権限を有効にしたことやエージェントの実行中表示だけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前にブロッカーを解消してください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の作業を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を学びました。
- 機能全体の PR と検証の証拠を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認しました。

次は[既存のキャンバスを使い、トリアージキャンバスを作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
