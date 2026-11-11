---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

アプリケーションの簡単な更新でも、複数のファイル変更やテストの実行が必要になることがあります。開発者は関連ファイルを探して変更し、テストを実行し、問題を調査して、変更漏れがあればさらに修正します。

Copilot Agent Mode は、統合開発環境 (IDE) の中でより自律的に作業します。プロジェクトを調べ、必要なファイルを更新し、テストを実行し、見つかった問題を自動的に修正します。この演習では、Copilot Agent Mode でサイトに機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途です。agent mode は IDE 内で動作し、短いフィードバック サイクルと対話を可能にします。cloud agent はチームの一員のように非同期で動き、issue とプルリクエストを通じて開発者とやりとりします。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、文書化の規約が Copilot にどう伝わるかを確認します。
- 現在の指示を使って、絞り込み用の最初のデータ スライス (パブリッシャー ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。
- 同じプロンプトを再実行し、生成コードが新しい規約を採用する様子を確認します。
- 次の演習で更新した指示を使えるよう、指示の変更と絞り込みの一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、出力の一字一句を一致させるのではなく、指示を更新した後の動作の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザー インターフェイス (UI) はダーク モードで、現代的な見た目にします。
- コードの文書化には TypeScript Documentation (TSDoc) コメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に伝えます。コーディング規約、ライブラリ、コメントの書き方などを指定でき、リポジトリ全体または特定のファイル種別に適用できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
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
- 自然な言葉を使い、指針を明確にして、コードの例を示します。

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

スターター プロジェクトに含まれる指示ファイルを確認してから変更を加え、影響を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングに関する指針を確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクト ファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、Copilot が例を参照できるようにします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内の文書化はまだ求めていません。次の手順で追加します。

## 変更用のブランチを作成する

変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。各演習で必要なモードとモデルを指定します。

まず、現在の指示ファイルを使ってプロンプトを送り、カスタム指示の影響を確認します。後で指示を更新した後、同じプロンプトを再送信して違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot に不要なコンテキストが渡らないようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストなどを更新する様子を確認します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもコメントを追加する場合があります。その場合も問題ありません。この演習では、指示の更新後に動作が一貫して改善することを確認します。

## リポジトリ全体に適用する規約を追加する

`.github/copilot-instructions.md` にプロジェクト全体の規約を追加し、コード提案の改善に役立てます。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクションを見つけます。27 行目付近にあります。既存の規約は記載されていますが、コード内の文書化に関する指示はありません。
3. 既存の規約の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体の指針を記載する `.github/copilot-instructions.md` と、言語、ファイル種別、タスクに応じた `*.instructions.md` のどちらにも作成できます。今回のようなプロジェクト全体の規約には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

更新した指示が Copilot のコード提案に反映されることを確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を導いて新しいプロジェクト規約に従わせ、次の演習で使うコードに適用しました。

## 指示と変更をコミットしてブランチをプッシュする

指示ファイルも他の資産と同様に、ソース管理で管理します。指示ファイルとヘルパーの変更をコミットして、リポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの基礎を作成するために Copilot が変更したファイルをステージしてコミットします。`git status` にテストなどの追加ファイルが表示された場合は、それらもステージします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の補助ファイル (テストなど) が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにある指示ファイルの更新に関する issue と関連付けて PR を作成します。GitHub ツールを使える Copilot に依頼します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** を選択します。
3. 指示ファイル更新の issue を探し、現在のブランチからプルリクエスト (PR) を作成するよう Copilot に依頼します。指示の更新と、今後の絞り込み作業に向けたパブリッシャー ヘルパーの追加の両方を PR に記載するよう伝えます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探し、PR を作成します。
5. Copilot に代わって GitHub 操作を許可する確認が表示されたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージとローカル ブランチの `main` への切り替えを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作やシェル コマンドの実行を許可する確認が表示されたら、操作を確認して **Allow** を選択します。

GitHub Copilot の支援を使って PR を作成し、マージしました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい規約が反映される様子を確認しました。
- 指示の更新とヘルパーの基礎をコミットして `main` にマージし、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに、これらの指示を[agent mode で適用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
