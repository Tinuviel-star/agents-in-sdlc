---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイル変更やテストの実行が必要になることがあります。開発者は関連ファイルを探して変更し、テストを実行し、問題を調査して、変更漏れがあればさらに修正します。

Copilot Agent Mode は、IDE の中でより自律的に作業します。プロジェクトの構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途です。agent mode は IDE 内で動作し、短いフィードバック サイクルと対話を可能にします。cloud agent はチームの一員のように非同期で動き、issue とプルリクエストを通じて開発者とやりとりします。

この演習では、次の方法を確認します。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更します。
- GitHub Copilot Agent Mode でユーザー インターフェイス (UI) とデータレイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリとパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャー ヘルパーを追加しました。次は Copilot Agent Mode を使い、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示する単一の Astro アプリで、ページはビルド時に静的な HyperText Markup Language (HTML) として生成されます。ページは frontmatter 内で Drizzle オブジェクトリレーショナル マッパー (ORM) を使ってローカル SQLite データベースを直接参照します。独立したバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブ機能には標準の Document Object Model (DOM) API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを起動するため、`npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行します。

1. Codespace に戻ります。前の演習の後、`main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のようなメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザー ウィンドウを表示するダイアログが開いた場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。Codespace で Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、現在の機能を確認します。

- **Home Page**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は手順で指定します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成したときに、issue のバックログも自動作成されています。GitHub Copilot にバックログを表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。エージェントは自律的に計画し、複数ファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。このワークショップは Claude Sonnet 4.5 を使って作成したため、手順に含めています。提案の一貫性を高めることが目的ですが、大規模言語モデル (LLM) は確率的に動作するため、例と異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が異なる Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって役立つと思われる順に項目を並べたことを確認します。

## 指示ファイルを確認する

コードの生成を始める前に、Copilot にコンテキストを与える指示ファイルを確認します。この演習では、サイトに機能を追加する方法を記載した [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル向け指示へのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。複雑な指示を再利用しやすい小さなファイルに分け、例やテンプレートを参照できます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つを更新します。

- データレイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込みの動作をテストするテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

コードベースにマージする前に、テストを実行して成功を確認します。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログ内の関連 issue に基づいて実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データレイヤー、UI、テストを特定し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを確認する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストなどを実行します。タスクやコードベースによって数分かかる場合があります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はありません。完了すると、変更ごとに **Keep** または **Undo** を選べます。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat にテスト実行の確認ダイアログが表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを反復し、エラーがないと判断するまで作業を続けます。

   ![Copilot Agent Mode のセッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する方法
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を依頼します。既存の指示ファイルなどのベスト プラクティスに従い、ページ サイズを利用者が選択できるようにするか固定にするかを検討します。希望する実装に必要な情報をプロンプトで伝え、必要に応じて Copilot に変更やコンテキストの追加を依頼します。これは Copilot と作業するときの通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
