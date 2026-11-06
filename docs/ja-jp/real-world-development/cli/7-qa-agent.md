---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、明確な QA プロセスを定義したカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証 (QA) プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の Issue に照らして報告内容をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントで QA プロセスを調整し、再利用可能なレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する、Copilot の専門的な役割です。プロファイルにはエージェントの目的、指示、利用できるツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの規約を示します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。既存の機能を置き換えたり、別の会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルには、QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送り、カスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと境界がプロファイルに含まれていることを確認します。これにより、検証だけを依頼したのに不完全または広範なエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、要件の確認、リポジトリ指示の遵守、`quality-checks` スキルの実行、Playwright MCP の利用が含まれていることを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. これらの責務や境界が欠けている場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を QA で確認する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルター機能の会話を再開し、QA を選択します。これにより、Issue と計画時の判断をコンテキストとして使えます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業が始まります。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに追加し、結果をレビューしました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な QA プロファイルを作成して調べました。
- QA エージェントを選択し、フィルター機能の Issue に照らして報告内容をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
