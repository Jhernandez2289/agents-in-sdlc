---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しましたが、大きな変更にはより堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから、Issue を起点に機能を実装し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージする、エージェントを使った開発プロセスを進めます。

> [!NOTE]
> 機能開発の間は、同じ会話とブランチを使います。通常はファイル種別ごとにブランチや PR を分けますが、ここでは主な概念に集中するため、簡略化しています。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot との会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリとパブリッシャーでゲームを絞り込める必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておきたい詳細があります。Plan モードで方針を決め、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本的な工程は変わりません。一般的な流れは次のとおりです。

1. 必要な作業を記述した Issue から始めます。
2. 実装内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れに沿っています。

この標準的な進め方に従うことで、AI が生成したコードも要件を満たし、手作業で書いたコードと同じレビューを受けられます。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前に開発者の入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能に取り組む前に、Codespace とリポジトリの状態を準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

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
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエストをコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に任せる前に機能の実装方針を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望する動作に基づいて回答します。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読み、内容を確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、Copilot に実装を依頼します。Autopilot はタスクが完了したと判断するまで、問題への対応を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って実装を繰り返し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があるため、先に計画をレビューしてください。依存関係が不足している場合やポートが競合している場合は、チェック完了と判断する前に問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の判断に照らし合わせます。
4. レビュー後、<kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードに問題がなければ、アプリを起動し、Codespaces が転送するポートからサイトを開いて動作を確認します。

1. Copilot に次のプロンプトを送り、アプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、パブリッシャー、およびその組み合わせによるフィルターを試します。
4. リセットと結果が空の場合の動作が、Issue と計画の判断に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。

- GitHub Issue から Copilot との会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示][next-lesson]を使い、コード生成が文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
