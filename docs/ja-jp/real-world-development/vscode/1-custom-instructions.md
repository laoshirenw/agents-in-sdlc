---
title: "エクササイズ 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、その情報を参照できるようにします。Copilot にコード生成の指針を伝える方法の1つが[指示ファイル][instruction-files]です。

このエクササイズでは、次の操作を行います。

- リポジトリのカスタム指示とファイル単位の指示を通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- 現在の指示を使って、絞り込み機能の最初のデータ部分 (出版社ヘルパー) を作成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 指示の更新と絞り込み機能の一部を `main` にコミットし、次のエクササイズで更新済みの指針を利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。このエクササイズでは、指示の更新前後で挙動がどのように変わるかを確認します。生成結果が文字単位で同じになることを期待するものではありません。

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

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** の指針も含まれます。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの glob で適用対象を定めます。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指針を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) と既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベストプラクティスに沿って、変更用ブランチを作成します。

1. 前のエクササイズで使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次のエクササイズを始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択はエクササイズごとに異なるため、必要な場合は手順内で説明します。

指示ファイルの影響を確認するため、まず現在の指示のままプロンプトを送ります。その後、指示を更新して、同じプロンプトをもう一度送ります。

1. 前のエクササイズで使ったエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に伝えます。
3. Chat ビューのエージェント選択欄から **Agent** を選び、ファイル変更を適用できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。ヘルパーとテストの両方を変更することもあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、出版社の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 生成されたコードには TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントが生成される場合があります。その場合も問題ありません。ここでは、指示の更新後に一貫性が向上することを確認します。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に提供します。コード提案の品質を高めるため、コーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在の標準には、生成されたヘルパーにドキュメントコメントがなかった理由となる規則がありません。
3. 既存の標準の直下に、ファイルヘッダーと TSDoc コメントを追加する次の Markdown を貼り付けます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体を対象とする `.github/copilot-instructions.md` と、特定の言語、ファイル形式、タスクを対象とする `*.instructions.md` のいずれかに置けます。追加したドキュメントコメント規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに集中できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次のエクササイズで使うコードに適用できました。

## 指示と変更をコミットしてブランチをプッシュする

指示ファイルも他の資産と同じようにソース管理します。指示の更新とヘルパーの変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新、ヘルパーの変更、および Copilot が関連して変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。GitHub ツールはすでにプロジェクトに接続されているので、Copilot に issue を探して PR を作成し、issue をクローズするよう依頼できます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは今回が初めてです。プロジェクトテンプレートには GitHub ツールが設定されていますが、初回利用時に VS Code から GitHub へのサインインを求められることがあります。画面の案内に従ってください。後のエクササイズで、この接続の仕組み (Model Context Protocol) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の変更と、今後の絞り込み機能で使う出版社ヘルパーの基盤の両方を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成するのを待ちます。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められたら、コマンドを確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の操作を行いました。

- 既存の指示を使ったコード生成プロンプトを送信しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次のエクササイズで利用できるようにしました。

次は、Copilot がコードベース全体に新機能を追加する[agent mode][next-lesson]で、指示ファイルを活用します。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のより良いカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
