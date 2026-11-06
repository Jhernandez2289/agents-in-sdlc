---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業方法に規則がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント規約を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに伝わる方法を確認します。
- コーディング規約を守るために指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインがあります。

- コメントには、コードの言い換えではなく、意図や自明でない判断を記載します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。該当する場合は注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変わった際はコメントを最新状態に保ちます。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発規約に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコード生成を促せます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、作業に関するガイドラインを記載します。

> [!NOTE]
> 指示ファイルの形式とサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには指示ファイルが用意されています。変更前に確認し、追加する指示の効果を比較できるようにします。

1. Codespace に戻ります。
2. Codespaces のエディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、コード生成の例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示ファイルは出発点として十分ですが、TypeScript に TSDoc コメントを追加する規則がありません。コアの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` を開き、35 行目付近にある **Code formatting guidance** セクションを探します。
2. セクションの最後に次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用し、機能のコンテキストを保つため、フィルター機能の会話を終了して再開します。

1. フィルター機能の会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新したガイダンスの適用を依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに伝わる方法を確認しました。
- コーディング規約を守るために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
