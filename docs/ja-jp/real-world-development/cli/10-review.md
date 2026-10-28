---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されている絞り込み issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み issue から作業を始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示を追加し、既存の `quality-checks` スキルを更新して、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込みを試しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザー上の証拠を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- 絞り込み機能の変更全体をレビューし、機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 実装したもの

ワークショップには、それぞれ更新済みの `main` から作成した独立ブランチで進める、2つの PR の節目があります。

1. **星評価**: ゲームカードに既存の `starRating` と未評価状態を表示します。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズして、QA プロファイルと関連テストを作成します。

計画から PR の作成までは同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を1つの PR にまとめています。

## 検証の種類

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果と要件のレビュー、テストカバレッジをまとめて PR の確認に役立てました。

追加のテストは、実際に不足している箇所を補うべきです。新たなテストが不要と QA が判断する場合もあります。マージを許可する前に、コードと証拠を確認し、変更後は関連する証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行する作業を記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は範囲を絞った変更を自分で確認しながら進めるとき、**Autopilot** は明確に範囲を定めたタスクに適しています。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

環境と同様に、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えることで、出力は大きく変わります。

## さらに試す

主要なワークフローを学びました。次の CLI 機能も確認してみましょう。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。製品コード、趣味のコード、何年も作ろうと思っていた小さなアプリなどで活用してください。チームと学びを共有し、お互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境を比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
