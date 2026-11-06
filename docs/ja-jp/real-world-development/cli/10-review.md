---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと登録済みのフィルター機能の Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能全体をレビューし、Agent Merge にフィルター機能の PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 成果物

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、関連作業を 1 つの PR にまとめています。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、更新した形式で結果を報告しました。QA はこれらの結果と、要件およびテストカバレッジのレビューをまとめました。

追加するテストは実際の不足を補うものであるべきです。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の根拠を確認し、変更後は影響する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズして、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は規約を定め、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を考えるとき、**Interactive** は対象を絞った変更に関与し続けるとき、**Autopilot** は範囲が明確なタスクを進めるときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力は大きく変わります。

## さらに確認する

主要なワークフローを終えました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。製品コード、趣味のコード、作りたいと思いながら手を付けていなかったアプリなどで活用してください。チームで知見を共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

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
