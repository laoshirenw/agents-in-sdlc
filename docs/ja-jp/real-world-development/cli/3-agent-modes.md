---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue に基づく絞り込みを実装し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しましたが、大きな変更にはより確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の作業フローに沿って動作し、必要なものを正しい方法で作れるよう支援します。このレッスンから数回にわたり、issue を起点に機能を作成し、コードの妥当性と動作を確認して、最終的にマージする一般的なエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能の作業は、同じ会話とブランチを使って続けます。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中するため簡略化しています。

このレッスンでは、次の操作を行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動確認します。

この後のレッスンでは、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリや出版社でゲームを絞り込みたいと考えています。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan モードで判断を確定し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記載した issue から始めます。
2. 実装計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって細部は異なりますが、多くの場合は上記の流れを調整したものになります。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ審査を受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと共同で作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードの作成、テストの実行、完了までの反復作業を自律的に行います。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

絞り込み機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てることで、機能の実装やタスクの実行を Copilot に任せる前に進め方を定められます。複雑な作業では、まず計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. 絞り込み機能の動作について質問された場合は、希望に沿って回答します。ここに唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画ができてレビューも完了したら、Autopilot を使って実装します。Autopilot は完了したと判断するまで、問題に対する反復作業を行います。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の項目を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行して反復する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まることがあります。必ず先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して正しく動作することを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、絞り込みの実装とテストを確認します。
3. 結果を issue と計画時の判断に照らし合わせます。
4. レビュー後、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がないように見えても、実行できるでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きましょう。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めた組み合わせを含め、カテゴリと出版社による絞り込みを試します。
4. リセットと結果が空の場合の動作が、issue と合意内容に一致することを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]、生成されるコードが文書化された開発慣行に従うようにしましょう。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
