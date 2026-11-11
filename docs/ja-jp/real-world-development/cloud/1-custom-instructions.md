---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が背景情報を必要とする場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot にタスクの内容だけでなく、実行方法も伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、文書化の規約が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのは困難です。後続の演習で、cloud agent が作成するプルリクエストをレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザー インターフェイス (UI) はダーク モードで、現代的な見た目にします。
- コードの文書化には TypeScript Documentation (TSDoc) コメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に伝えます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル種別に対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、Copilot CLI やチャットへの多くのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に対する指針を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、新しいユニットテストの作成などを対象にできます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。詳細は[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルを詳しく作成する方法はこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、開発対象やプロジェクトの構成、全体的なコーディング規約など、プロジェクト全体に関する指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指針を記載します。
- 自然な言葉を使い、明確な指針とコードの例を示します。

指示ファイルや AI の使い方に唯一の正解はありません。プロジェクトに合った方法を試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルを確認できます。
>
> Copilot に指示ファイルを作成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で方法が異なります。必要な場合は、使用中の環境に対応するレッスンで手順を確認してください。
>
> テンプレートやひな形を探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

プロジェクトに用意された主要な指示ファイルと、タスク別の `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針は、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドがリポジトリのルートからの glob を指定し、例として `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されることを確認します。
5. プロジェクト固有のユニットテスト作成指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイルや既存ファイルへのリンクを確認します。これにより、指示を再利用可能な小さなファイルに分け、Copilot がコード生成時に例を参照できます。ここにあるパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの規約が記載されていますが、コード内の文書化はまだ求めていません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリ全体に適用する規約を追加する

次に、コード内の文書化を TSDoc コメントとファイル レベルのコメント ヘッダーで行うよう、プロジェクト全体の規約を追加します。後で issue を割り当てたときに、cloud agent がこの規約を使います。

1. Codespace のターミナルで、作業用のブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. **Code formatting requirements** セクションを見つけます。セクションは 27 行目付近にあります。既存の規約は記載されていますが、コード内の文書化に関する指示はまだありません。
4. 既存の規約の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体の指針を記載する `.github/copilot-instructions.md` と、言語、ファイル種別、タスクに応じた `*.instructions.md` のどちらにも作成できます。今回のようなプロジェクト全体の規約には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチにある指示ファイルを読み込みます。次の演習では `main` からブランチが作成されるため、cloud agent が指示を使えるよう、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への PR を開き、マージします。次の演習で issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの手順を省略して `main` に直接コミットできます。ここでは実際のプロジェクトでの手順に近づけるため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。残りのハーネスで cloud agent が作成する PR にも、この規約が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
