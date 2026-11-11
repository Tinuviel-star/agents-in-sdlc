---
title: "演習 3 - Playwright Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode で絞り込み機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。**Playwright MCP サーバー**を接続すると、Copilot に実際のブラウザーを操作させて機能をテストできます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーで絞り込み機能を手動テストさせます。
- ブランチを公開し、絞り込み作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にあるツールを利用すると、さらに便利になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールとのやりとりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作を実行したりできます。

ツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールとの通信を管理します。たとえば、既存の API や npm パッケージなどのローカル ツールに接続できます。各 MCP サーバーは、AI エージェントがアクセスできるツールやリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。リポジトリの作成や更新、issue やプルリクエストの管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などを AI エージェントが実行できます。

他にもさまざまなツールやリソースを提供する MCP サーバーがあります。GitHub はエコシステムの発見や貢献を促す [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーもプロジェクト内の他の依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供し、機能のテストに使えます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次の内容にします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は Playwright に画面を表示せずにブラウザーを実行するよう指定します。デスクトップ画面がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使うため、ブラウザーは通常すでにインストールされています。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを使うと、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されたら選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するよう求められた場合は、構成を確認して、信頼する選択肢を選びます。

手動での起動は任意です。省略した場合は、次の手順で Copilot が初めてツールを必要としたときに、VS Code がサーバーを起動し、信頼を確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを手動で操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を確認できます。その内容を報告させることで、会話を離れずに機能をすばやく検証できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素を調べます。支援技術と同様の方法で操作するため、基本的な機能確認と同時にアクセシビリティも簡単に確認できます。

サーバーを接続してアプリを実行した状態で、Copilot に次の手順を試すよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、手順を試して結果を報告します。報告を issue の受け入れ条件に照らし合わせます。問題があれば、PR を開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` で実行中のアプリが必要です。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームのレビュー用に PR を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が変更したファイルを表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、PR を作成する方法はいくつかあります。ここでは GitHub Copilot に issue を見つけさせ、関連付けて PR を作成します。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンから **Agent** を選択し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot に情報収集や操作の実行を許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは認証を求められる場合があります。表示される手順に従います。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が表示したリンクを選択して PR をレビューします。まだマージしないでください。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで絞り込み機能を手動テストするよう Copilot に依頼しました。
- ブランチを公開し、絞り込み作業の PR を作成しました。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどに使える[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
