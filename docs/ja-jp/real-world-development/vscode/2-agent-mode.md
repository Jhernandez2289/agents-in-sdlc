---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリケーションの変更でも、複数のファイルを更新し、テストを実行する必要がある場合があります。開発者は必要なファイルを探して変更し、テストを実行して問題を調査し、変更漏れがあればさらに修正します。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、プロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる利用体験を提供します。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整する方法。
- UI とデータ層にまたがる新機能を GitHub Copilot エージェントモードで実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。ここでは、Copilot エージェントモードを使って、残りのデータ層、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは Drizzle ORM を使って frontmatter からローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアントサイドのユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクで、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習の後に `main` に戻っていることを確認し、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、codespace 内に新しいターミナルを開きます。
3. フィルター機能の作業用ブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストする codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の作業を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成すると Issue のバックログも自動作成されるため、GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェント選択メニューで **Agent** を選択します。エージェントは変更を計画・実装し、ターミナルコマンドやツールを実行できます。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案をできるだけ一貫させることが目的ですが、LLM は確率的に動作するため、説明と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことも確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加するときの方針を記載した [user interface (UI)][ui-instructions] ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するときの全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネント設計・テスト容易性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル種別に関する指示へのリンクが含まれています。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きな指示を小さく分け、例やテンプレートを示すのに便利です。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) の残りのフィルター処理を追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

マージする前にテストが成功する必要があります。Copilot エージェントモードに作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。
4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ層ヘルパー、UI、テストなど関連ファイルを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行などを行うため、作業には数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。Copilot が終了するとすべての変更に対して **Keep** または **Undo** を選べるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。
8. 作業中に一時停止したりテストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーが検出されなくなるまで修正します。
9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- GitHub Copilot エージェントモードで、UI とデータ層にまたがる機能を実装しました。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定して、複数の変更を調整しました。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 - ページングを実装する

ゲーム一覧が増えるとページングが必要になります。この演習で学んだことを使い、Copilot にページングを追加してもらいます。既存の指示ファイルを使う、ページサイズをユーザーが選ぶか固定するかを決める、期待する実装に必要な情報をプロンプトに含めるなどを検討してください。Copilot に変更を依頼し、コンテキストを追加しながら反復するのは通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
