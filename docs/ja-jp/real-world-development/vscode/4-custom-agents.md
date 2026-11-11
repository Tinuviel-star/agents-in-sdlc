---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフローの特定のタスクや分野に合わせて作成する、専門的な人工知能 (AI) アシスタントです。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置くと、Copilot に焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を与えられます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなど、チームの専門知識を再利用できるエージェントにできます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキルや他のエージェントを使って作業を調整します。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローやテストの実行と管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI の他の使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、現在のダーク テーマでは文字と背景のコントラストが十分でなく、読みにくい利用者がいます。デザイン チームは、利用者が切り替えられる高コントラスト モードを求めています。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。内容を確認して、Copilot の作業にどう反映されるかを把握します。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します (Exercise 3 の絞り込み PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきこと

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したカスタムエージェントが Copilot Chat ビューのエージェント ドロップダウンに表示されます。エージェントを選択すると、その指示とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は手順で指定します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. チャット ビューの下部にあるエージェント ドロップダウンを選択します。既定では、現在のエージェントとして **default** が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集する様子を確認します。各編集はファイル パスと差分とともにチャットに表示されます。
6. 編集が追加されると、**Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集し、リポジトリに実際の変更を加えています。

次の演習では、作業中のセッションをレビューし、追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や規約を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

この演習では、次の操作を行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタムエージェントを使いました。

次は、エージェントの作業を[監視して指示を追加します][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
