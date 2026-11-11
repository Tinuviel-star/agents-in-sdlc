---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で issue に基づく絞り込みを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、もっとしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて使えます。このレッスンから、issue に基づいて新しい機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージする、一般的なエージェント主導の開発プロセスに進みます。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常は作業対象ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub issue から Copilot の会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動確認します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な詳細があります。Plan モードで決定し、Autopilot に範囲を定めた実装を許可します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。一般的な流れは次のとおりです。

1. 作業内容を記載した issue から始めます。
2. 実装内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くは上記の流れを基本にしています。

この標準的な方法を守ることで、AI が生成したコードも、手作業で書いたコードと同じレビューと検証を受けられます。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了したと判断するまで自律的に反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

絞り込みの作業を始める前に、Codespace とターミナルの準備を確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、まず計画を立てることをお勧めします。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. 絞り込みの動作について質問されたら、希望する動作を回答します。選択は開発者が決められます。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 決めた内容と異なる箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot で実装します。Autopilot を使うと、Copilot はタスクが完了したと判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストールしたバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。Copilot から依存関係の不足やポートの競合が報告された場合は、チェック完了と判断する前に解決してください。

## 実装をレビューして検証する

生成されたコードは、マージ前に他のコードと同様にレビューします。コードを確認し、サイトを実行して動作を検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、絞り込みの実装とテストを確認します。
3. 結果を issue と計画時の決定事項に照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新しい機能を確認する

コードを確認したら、Codespaces の転送ポートを使ってアプリを起動し、サイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、パブリッシャー、およびその組み合わせで絞り込みを試します。
4. リセット時と結果がない場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を作成しレビューしました。このレッスンでは、次の操作を行いました。

- GitHub issue から Copilot の会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動確認しました。

次は、[カスタム指示を使って][next-lesson]コード生成の動作を深く確認し、文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
