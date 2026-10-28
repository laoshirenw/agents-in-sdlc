---
title: "エクササイズ 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

このエクササイズでは、次の操作を行います。

- リポジトリのカスタム指示とファイル単位の指示を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、このエクササイズでは指示更新前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成するプルリクエストをレビューするときに指示の影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードで、現代的な見た目にします。
- コードのドキュメントには TSDoc のコメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示では、コーディングスタイルや要件などを Copilot に伝えられます。規約、ライブラリ、コメントの書き方などを指定することで、より適切な提案を得られます。リポジトリ全体、または特定のファイル形式やタスクに対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリ内のすべてのリクエストで Copilot に渡す、単一の指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティス、全般的な指針など、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル形式に対するガイドラインを記載します。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、別の規約による指示ファイルにも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細はここでは扱いませんが、サンプルプロジェクトには代表的な構成が含まれています。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全般的なコーディング標準など、プロジェクト全体に関する情報を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなど、ファイル形式やタスク固有の指示を記載します。
- 自然な言葉で、明確な指針とコードの良い例・悪い例を示します。

指示ファイルを作成する唯一の正解や、AI の唯一の利用方法はありません。プロジェクトで試しながら、最適な方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境によって方法が異なります。現在の環境で利用できる方法は、各レッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。GitHub の Web UI または起動済みの Codespace で開きましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** の指針も含まれます。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` frontmatter は、リポジトリのルートからの glob で適用対象を定めます。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指針を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) と既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 新しいリポジトリ標準を追加する

このエクササイズで編集するのは、cloud agent がコード内に TSDoc コメントとファイルレベルのコメントヘッダーを追加するよう、プロジェクト全体の規則を加えることです。後のエクササイズで issue を割り当てたときに、cloud agent がこの規則を使います。

編集する前に、作業用のブランチを作成します。cloud agent は issue が対象とするブランチから指示ファイルを読み込みますが、変更をきちんとコミットします。

1. Codespace のターミナルでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト全体の指針を記載しているので、そこにコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在の標準には、生成されたヘルパーにドキュメントコメントがなかった理由となる規則がありません。
3. 既存の標準の直下に、次の Markdown を貼り付けます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体を対象とする `.github/copilot-instructions.md` と、特定の言語、ファイル形式、タスクを対象とする `*.instructions.md` のいずれかに置けます。追加したドキュメントコメント規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチから指示ファイルを読みます。次のエクササイズでは issue を `main` を対象に割り当てるため、cloud agent が指示を利用できるよう、変更を `main` にマージする必要があります。

1. ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. GitHub.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。これで次のエクササイズで cloud agent が指示を読み込めるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して `main` に直接コミットできます。ここでは実際のプロジェクトに近い手順を練習するためにブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しいプロジェクト標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストを確認するときに標準が使われていることを確認します。

次は、最初の issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のより良いカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
