---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チームが改善を検討する関連ゲーム機能の作成、高コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。生成されたコードを確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督なしで非同期に作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みはコード用ブランチに限られます。
- cloud agent は GitHub Actions 内に、独立した一時的な環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールが GitHub Copilot をブロックしたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、ドキュメントの追加を示すタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があるため、その間に休憩したり、これまでに学んだことを振り返ったりできます。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントなど、追加されたドキュメントを確認します。具体的な変更内容は異なります。Exercise 2 の最初に確認したカスタム指示ファイルに基づく TSDoc コメント、TypeScript の規約、コメント ヘッダーにも注目します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されることを確認します。単一 Astro アプリのプロジェクト チェックが成功するまで待ちます。数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot と PR 上でやりとりできます。チーム メンバーへのコメントと同じように、PR やコード行のコメントで Copilot にメンションすると、別のセッションが起動して変更を依頼できます。結果は非決定的なため、必ず同じ文章を使う必要はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントの変更を **@copilot** に依頼するコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで反復できます。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームの表示を示すタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を探します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが追加されることを確認します。単一 Astro アプリのプロジェクト チェックが成功するまで待ちます。数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動で試せます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。cloud agent には、表示する関連ゲームの数の調整や、コメント ヘッダーと TSDoc コメントの追加など、追加の変更を依頼できます (この作業はカスタム指示の更新**前**に依頼したことを思い出してください)。**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントが実装した機能を確認します。この PR には、Exercise 3 で依頼した高コントラスト モードと、Exercise 4 で mission control から追加を依頼したライト モードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、高コントラスト モードを示すタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了するまで数分待つことがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 特に次の実装を確認します。
   - モード切り替え用のユーザー インターフェイス (UI) コンポーネント
   - 利用者の設定を保存する local storage の使用
   - 高コントラスト モードとライト モードの CSS またはスタイル変更
   - Accessible Rich Internet Applications (ARIA) ラベルやキーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されることを確認します。単一 Astro アプリのプロジェクト チェックが成功するまで待ちます。数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動で試せます。Codespace のターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、高コントラストとライト モードの切り替えが動作し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタムエージェントが、アクセシビリティのベスト プラクティスに沿った実装を Copilot に促したことを確認します。改善点があれば、前の PR と同様にコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — さらに作業を委任する

cloud agent にはバックログのタスクを任せ、その間に別の作業に取り組めます。リポジトリに issue を追加して Copilot に割り当て、この方法に慣れてください。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次の操作を行いました。

- リポジトリに含まれるカスタム指示を確認し、cloud agent の出力に与える影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使用し、高コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して指示を追加しました。
- Copilot の PR をレビューして反復し、**@copilot** に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスでは、今回の内容を別の環境で実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次の案を試せます。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てます。
- 別の分野に特化したカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
