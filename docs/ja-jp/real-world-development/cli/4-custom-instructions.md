---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、絞り込みコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用するときは、コンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。絞り込み機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用しましょう。

このレッスンでは、次の操作を行います。

- リポジトリの指示とファイル単位の指示が、エージェントにどう適用されるかを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- コメントでは、コードの内容を繰り返すのではなく、目的や判断の理由など、自明でない点を説明します。
- `db/` と `src/lib/` のエクスポート関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。`db` 引数を注入できる場合は、その引数も文書化します。
- 再利用可能な Astro コンポーネントは `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの慣行に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件などのコンテキストを Copilot に提供できます。好みの規約、ライブラリ、コメントの書き方などを指定でき、より適切な提案を得るのに役立ちます。リポジトリ全体、または特定のファイル形式やタスクを対象に指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に渡される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスク向けの指針を記載します。

> [!NOTE]
> 他の指示ファイル形式への対応状況は環境によって異なります。形式を利用する前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示を確認する

スタータープロジェクトには、すでに指示ファイルが含まれています。変更を加える前に、既存の内容とその影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要と開発上の指針を確認します。この指示は、リポジトリ内で Copilot とやり取りするたびに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームの方針に合わせて指示ファイルを更新する

既存の指示にはまだ足りない部分があります。新しく生成する TypeScript に TSDoc コメントを含めるため、`copilot-instructions.md` を更新しましょう。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35行目付近) を探します。
2. 最後の箇条書きに次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用するには、機能のコンテキストを失わないよう、絞り込みの会話を終了して再開します。

1. 会話が開いている場合は `/exit` で終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチからリポジトリのルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   必要であれば、絞り込みの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明が正確であることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを参照する仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とファイル単位の指示が、エージェントにどう適用されるかを確認しました。
- コーディング標準を守るために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
