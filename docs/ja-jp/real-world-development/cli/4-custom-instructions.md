---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、文書化の規約を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も指定できます。絞り込み機能を実装したので、Copilot が参照した指示を確認し、文書化の規約を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルが、エージェントにどう適用されるかを確認します。
- 指示ファイルを更新し、コーディング規約に従うようにします。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- コメントでは、コードの説明を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TypeScript Documentation (TSDoc) / JavaScript Documentation (JSDoc) コメントを記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint の指示を維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供します。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に伝えます。コーディング規約、ライブラリ、コメントの書き方などを指定でき、リポジトリ全体または特定のファイル種別に適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。多くのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関する指針を記載します。

> [!NOTE]
> その他の指示形式とサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更を加える前に、既存の指示を確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングに関する指針を確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、Copilot が例を参照できるようにします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting guidance** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内の文書化はまだ求めていません。次の手順で追加します。

## 指示ファイルを更新してチームの規約を反映する

`.github/copilot-instructions.md` に TypeScript の文書化規約を追加し、Copilot がコードを生成するときに適用されるようにします。

1. `.github/copilot-instructions.md` を開きます。
2. 35 行目付近にある **Code formatting guidance** セクションを見つけます。
3. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指示を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を使うには、絞り込みの会話を終了して再開します。これにより、機能のコンテキストを保ったまま、新しい指示を読み込めます。

1. 絞り込みの会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

3. 求められた場合は絞り込みの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
4. 次のプロンプトを送信し、生成したコードを更新した指示に合わせるよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、機能に新しい規約を適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルが、エージェントにどう適用されるかを確認しました。
- 指示ファイルを更新し、コーディング規約に従うようにしました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
