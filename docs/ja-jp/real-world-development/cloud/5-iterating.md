---
title: "エクササイズ 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot とともに、ユーザー体験の改善と機能追加を行ってきました。コードにドキュメントを追加し、デザインチーム向けに反復改善できる関連ゲーム機能を実装し、高コントラストとライトモードの切り替えなどアクセシビリティ機能を追加するよう依頼しました。コード変更を確認し、必要に応じて Copilot にフィードバックを送って、作業を改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視されていない状態で作業するため、安全性を確保する制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスは作業ブランチに限定されます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなど外部リソースへのアクセスが既定で制限されているため、この警告は**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業完了まで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc コメントやその他のドキュメントを確認します。具体的な変更内容は異なる場合があります。レビュー時には、エクササイズ 2 の指示ファイルで確認した TSDoc コメント、TypeScript 規約、コメントヘッダーに注目してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、それに対応する新しいセッションを開始します。結果は確率的に変わるため、決まった依頼文は示せません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの内容を簡潔に説明するコメントヘッダーを追加します。
- TypeScript と Astro ファイルに TSDoc コメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. **@copilot** をユーザーと同じようにメンションし、生成されたドキュメントの変更を依頼するコメントを追加します。上記の例から選ぶか、コードベースのドキュメントに関して別の変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は変更用に新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで改善を続けます。完了したら、PR をドラフトからレビュー可能な状態にし、`main` にマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を実装しました。ほかの PR と同様に、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます (この issue はカスタム指示を更新する前に割り当てたことを思い出してください)。**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントで実装した機能をレビューします。この PR には、エクササイズ 3 で依頼した高コントラストモードと、エクササイズ 4 で Mission control から追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで数分待つことがあります。

4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モード切り替え用の UI コンポーネント。
   - 利用者の設定を保存するローカルストレージの使用方法。
   - 高コントラストモードとライトモードの CSS またはスタイルの変更。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace で Copilot が作成したブランチに切り替えて、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、ブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   次にアプリケーションを起動し、ブラウザーで高コントラストモードとライトモードの切り替えを試します。ページを再読み込みしても設定が保存されることを確認します。

アクセシビリティカスタムエージェントが、ベストプラクティスに沿って機能を実装するよう Copilot を導いたことが分かります。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意のエクササイズ: 追加タスクを委任する

Cloud agent は、バックログの実際の issue を渡して別の作業に取り組めるときに効果を発揮します。習慣にするため、リポジトリにいくつか issue を作り、Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを追加します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次の操作を行いました。

- **このリポジトリにあるカスタム指示ファイルを確認し**、cloud agent の成果に反映される指示を学びました。
- **issue を Copilot cloud agent に割り当て**、環境を準備し、計画して、非同期に作業する様子を確認しました。
- **アクセシビリティ向けのカスタムエージェントを作成して使い**、高コントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティセッションを監視して導きました。
- **cloud agent のプルリクエストをレビューして改善し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスでは、学んだ内容を別の環境で実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
