---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様、GitHub Copilot CLI には操作用のスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定項目にアクセスできます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも見てみましょう。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えたので、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、手動で確認や圧縮もできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。タイミングを指定したい場合は `/compact` を使います。関係のない別タスクを始める場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルによって得意な作業は異なり、開発者ごとに好みも異なります。Copilot CLI では、利用可能なモデルを確認して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、全員の AI ツール活用に役立ちます。`/share` コマンドは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンに表示される `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポート内容を確認してから、送信または公開します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業の拡張と委任

ワークショップの主要部分は完了です。Copilot CLI には、大きなタスクに使える次のコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信します。エージェントは非同期で作業し、プルリクエストを開くことがあります。

これらのコマンドは、追加の worktree やリモート作業を作成することがあるため任意です。試すときは、範囲の明確な新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントについて詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定やセッションの共有、内部状態の確認ができます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認しました。

Copilot CLI には、さらに多くのスラッシュコマンドがあります。学んだことと次のステップを[振り返りましょう][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
