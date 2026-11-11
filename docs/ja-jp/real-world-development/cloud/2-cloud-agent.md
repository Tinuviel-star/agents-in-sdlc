---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト プルリクエスト (PR) を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確に定めた issue の重要性を確認します。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、最初のサイトを開発した委託先が残した技術的負債があります。コードの文書化がまったくないため、まずアプリケーションのエクスポート関数に TypeScript Documentation (TSDoc) コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリの他のゲームを表示し、利用者が閲覧を続けられるようにします。現時点では完成度の高い実装は必要なく、ユーザー エクスペリエンス (UX) の受け入れテストに使える状態を目指します。現在は作業の妨げになっていますが、より優先度の高い issue もあります。

このようなタスクは優先順位が下がりやすいため、Copilot cloud agent に適しています。非同期で作業を任せている間、開発者は別のタスクに取り組み、後で成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業を実行できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当て後、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取りかかります。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、PR で開発者に通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に任せれば考える必要がなくなるわけではありません。一見簡単な作業でも、詳細を掘り下げると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示が Copilot をどう導くかを確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、この機会に確認してください。cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後で影響を判断できます。

リポジトリの GitHub Web ユーザー インターフェイス (UI)、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TypeScript Documentation (TSDoc) コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` に指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認します。後の演習で PR をレビューするときに、改めて注目します。

## Copilot cloud agent の開発環境を準備する

コードの作成には、通常、環境やセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を準備できます。このラボ用にあらかじめ設定されており、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用に移行およびシードされた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は独自のトークンを使います。手順で必要となる最小限の権限を設定してください
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースを移行してシードし、ビルドとテスト用のデータを準備します
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同様ですが、次の点に注意してください。

- `copilot-setup-steps` という 1 つのジョブがあります。Copilot が PR の作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でワークフローを実行し、動作を確認できます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いままになっていたり、まったくなかったりします。この種の技術的負債は見過ごされやすく、生産性を下げ、コードの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成にも役立つため、cloud agent に依頼するのに適したタスクです。後の演習で生成された変更をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![issue への Copilot 割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot が todo リストを作成するまで数分かかる場合があります。前述のワークフローで環境を準備し、プロジェクトを分析して、作業方法を検討しています。

13. todo リストと実行予定のタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ページを更新してください。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調べて状態を理解し、方針を考え、コードを作成する様子を確認できます。

作業には数分かかる場合があります。cloud agent の主な利点は、作業を非同期で進め、開発者が別のタスクに取り組めることです。次は別のタスクも cloud agent に割り当て、その間にアプリケーションの機能を追加します。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent を使うと、作業を分担して開発者が別のタスクに取り組めます。デザイン チームのために関連ゲームの表示を追加する作業を、cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![issue への Copilot 割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

ほどなくして、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを示します。

![目の絵文字で Copilot が issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。ソフトウェア エンジニアと同じように動作するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当てると、非同期で作業を依頼できます。技術的負債の解消、新機能の作成、フレームワークの移行などに活用できます。

この演習では、次の概念を確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確に定めた issue の重要性
- Copilot への issue の割り当て

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用を確認します。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも使えます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
