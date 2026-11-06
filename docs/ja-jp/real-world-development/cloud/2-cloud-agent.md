---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織は、未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent で次のことを確認します。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確に定めた Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。Web サイトの初期版を作成した開発者は、ドキュメントを十分に整備しませんでした。まず、アプリケーションのエクスポートされた関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する別のゲームを表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使える状態にすることが目的です。この作業はブロッカーですが、優先度がより高い Issue もあります。

どちらも優先順位が下がりやすい作業であり、Copilot cloud agent に任せるのに適しています。非同期で作業させている間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる][assign-issue]などの方法があります。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に任せても、考える作業を省略できるわけではありません。単純に見える操作にも、詳しく調べると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペアプログラマーとして扱い、段階的に進め、試し、結果に合わせて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、cloud agent に作業を割り当てる前に、リポジトリに用意された指示ファイルを読み取り専用で確認してください。後で結果をレビューするときに、指示の影響を見つけられます。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを確認します。

- `.github/copilot-instructions.md` の **Code standards** セクションを読み、TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターが `**/*.test.ts` に指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているかを確認します。後の演習でもレビュー時に取り上げます。

## Copilot cloud agent の開発環境を準備する

コードを作成する際は、通常、適切な状態を保つための環境やセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェアエンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を用意できます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、マイグレーションとシードが済んだ SQLite データベースを利用できるよう、あらかじめ設定されています。このプロジェクトは単一の Astro アプリです。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同様ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の作業を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、情報が古かったり不足していたりするプロジェクトは少なくありません。こうした技術的負債は対応されずに残り、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に割り当てるのに適した Issue です。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` などです。まだ表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> TODO リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し、前述のワークフローを実行し、プロジェクトを分析して、作業の方法を決めています。

13. TODO リストと Copilot が実行する作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールし、Copilot がコードを調べて状態を理解し、方針を検討してコードを作成する様子を確認します。

この作業には数分かかる場合があります。cloud agent の主な目的の 1 つは、非同期にタスクを実行して開発者がほかの作業に集中できるようにすることです。この機能を使い、もう 1 つのタスクを cloud agent に割り当ててから、アプリの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

作業を分担できることは、GitHub Copilot cloud agent の利点です。デザインチーム向けに関連ゲームを追加する作業自体は短時間で終わるかもしれませんが、その時間をほかの作業に使えます。cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。cloud agent はソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後にレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使うと、Issue を Copilot に割り当てて非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
