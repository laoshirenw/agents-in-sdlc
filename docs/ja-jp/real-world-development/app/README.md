---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発に集中できる単一のワークスペースを提供します。並列エージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub issue とプルリクエストのネイティブ管理機能を備えています。さらに **Agent Merge** を使うと、プルリクエストの rebase、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージをエージェントに任せられます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. 絞り込み issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、絞り込みの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込み機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、検証の証拠を確認します。
8. 絞り込み機能の変更全体をレビューし、2つ目の PR に Agent Merge を使います。
9. キャンバスが利用できるか確認してから Database Explorer を使用し、リポジトリに保存するトリアージキャンバスを作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップの焦点を絞るため、PR は2つ作成します。1つ目は星評価、2つ目は絞り込み機能と指示・スキル・QA プロファイル・テストの更新です。各 PR は更新済みの `main` から開始します。絞り込みと品質のワークフローでは1つのセッション、worktree、ブランチを共有するため、各ツールを学びながら前の作業を活用できます。最後のキャンバス演習はそのセッション内で行い、PR ワークフローを繰り返さず、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、PR 1 をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で実装して、Interactive でレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認・更新して、絞り込みに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存スキルを確認し、レポート形式を変えて実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize で MCP を設定し、絞り込みの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家のプロファイルを作成して選択し、検証の証拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 絞り込み、指示、スキル、QA プロファイル、テストをレビューし、2つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを振り返ります |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランが設定された GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者は **GitHub Copilot app** ポリシーを有効にしておく必要があります。この設定は Copilot CLI ポリシーとは別です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
