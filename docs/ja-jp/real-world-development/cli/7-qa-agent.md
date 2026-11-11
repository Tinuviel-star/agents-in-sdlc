---
title: "レッスン 7 - 品質保証 (QA) エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright Model Context Protocol (MCP) でブラウザー上の絞り込み機能を確認しました。次は、明確な品質保証 (QA) プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次の操作を行います。

- カスタムエージェントが指示、スキル、MCP ツールをどう使うかを確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選び、絞り込み issue に基づく調査結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントが QA プロセスを調整し、再利用可能な報告を作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルにはエージェントの目的、指示、使用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの規約を説明します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、これらの機能をどう使うか Copilot に伝えます。既存の機能を置き換えるものでも、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成させます。プロファイルには、QA が実行するチェックと守るべき境界の両方を記載します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、Copilot が意図した QA の手順と制約を反映したことを確認します。検証だけを依頼したのにプロファイルが不完全だったり、変更範囲が広すぎたりすることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は任意ですが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、要件を起点にして、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認を求め、コミットや PR の作成をしないことを確認します。
5. これらの役割や制約が欠けている場合は、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じ絞り込みの会話を再開して QA を選択すると、issue と計画時の決定を利用できます。

1. 次のコマンドでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、選択可能な一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 完了したら、提示された報告を読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その作業をレビューしました。このレッスンでは、次の操作を行いました。

- カスタムエージェントが指示、スキル、MCP ツールをどう使うかを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、絞り込み issue に基づく調査結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証報告が揃いました。次は、これらを[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
