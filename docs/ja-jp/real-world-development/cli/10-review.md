---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みの絞り込み issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーで確認した根拠を評価しました。
- 絞り込みの変更全体をレビューし、Agent Merge に絞り込み PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップには、更新した `main` からそれぞれ別のブランチで作成する 2 つの PR があります。

1. **星評価**: 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` の報告形式を変更し、QA プロファイルと関連テストを追加します。

絞り込みの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP 経由の Copilot によるブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、更新した形式で結果を報告しました。QA はこれらの結果と要件およびテスト範囲のレビューをまとめて PR の確認に役立てました。

追加するテストは、実際に不足している範囲を補う必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響する根拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの構成を行いました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は規約を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** で実装前に方針を検討し、範囲を絞った変更には **Interactive** で作業を確認しながら進め、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI には、ほかにも役立つ機能があります。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。製品コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
