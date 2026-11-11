---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして絞り込み機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードを検証し、指示ファイルで規約を守り、テストや lint などの継続的インテグレーション (CI) を実行する必要があります。

このような繰り返し作業には、**エージェント スキル**が適しています。スキルを使うと、こうした操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は `quality-checks` スキルをユニットテスト、lint、型チェックに使います。チームでは、結果を読みやすくするために報告を改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめます。基本構成は、スキル名のフォルダーと、その中の `SKILL.md` という Markdown ファイルです。Markdown には、スキルの内容を定義する `name` と `description` の frontmatter、処理の概要、呼び出すタイミングの指針を記載します。スキルのフォルダーには、呼び出し時にエージェントが使うスクリプトやその他のリソースを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` のみで構成されています。

スキルはプロジェクトの `.github/skills` に置いて、チームで共有し再利用できます。またはユーザー スキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。description は Copilot がスキルを呼び出すタイミングを判断するために使います。
3. 指示を読み、Copilot がテストと lint を実行する方法を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。まず、3 種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードの絞り込み会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示される報告を確認します。

## 報告をカスタマイズする

何を実行したか、成功したか、ツールがどのような結果を報告したかが分かるように、スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルを保存します。変更は自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行して動作を確認します。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後の報告を確認し、最初の報告と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして実行しました。このレッスンでは、次の操作を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は絞り込み機能とともに機能 PR に含まれます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## その他のスキル例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図とエクスポート スクリプトを含む `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
