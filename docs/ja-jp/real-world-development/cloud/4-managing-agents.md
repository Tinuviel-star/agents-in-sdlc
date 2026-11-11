---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー エクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつもあります。この mission control では、オープンなプルリクエスト (PR) があるエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

エージェントに高コントラスト モードを依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイル更新と切り替え機能をすでに作成しているため、高コントラストに加えてライト モードも実装するようエージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の一覧にあるタスクを確認します。次のようなタスクが表示されます。
   - コードベースのドキュメント更新
   - ゲーム変更用 API の生成
   - Web サイトへの高コントラスト モードの追加
3. 実行中のタスクを 1 つ選択し、Copilot が行った操作を確認します。次のような操作が含まれます。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - Model Context Protocol (MCP) サーバーのセットアップ
   - 割り当てられたタスクを完了するための各種操作

> [!NOTE]
> 表示される手順は Copilot の作業状況や実装方法によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したら、高コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. 高コントラスト モードのタスクに対応するセッションを選択します。タイトルは Copilot が付けた名前や作業の状態によって異なります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで、作業中の cloud agent に追加指示を出すダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼を作業に追加します。

この処理には数分かかります。その間に、これまでに学んだことを振り返ってください。

## まとめと次のステップ

Copilot cloud agent の監視と指示に使う中心的な場所である Copilot agents ページを確認しました。この mission control では、進行中および完了したタスクを追跡し、実行中の作業をレビューし、進行中のセッションに追加の指示を与えられます。

このレッスンでは、次の操作を行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は[Copilot が作成した PR を反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
