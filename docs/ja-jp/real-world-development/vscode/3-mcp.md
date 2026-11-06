---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作して機能をテストしてもらいます。その後、ブランチを公開してプルリクエストを作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に MCP サーバーを使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールへアクセスすると、さらに強力になります。Model Context Protocol (MCP) は Copilot が外部ツールとやり取りするための標準的な方法です。

![エージェントモードとコンテキスト、大規模言語モデル、MCP サーバーや VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を行ったりできます。

MCP サーバーは AI エージェントと外部ツールやサービスの間を仲介します。既存の API や NPM パッケージのようなローカルツールとの通信を管理し、エージェントが利用できるツールやリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスし、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを行います。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作し、Web ページへの移動、フォームの入力、ボタンの選択などを行います。

さまざまなツールやリソースを提供する MCP サーバーがあります。GitHub の [MCP registry](https://github.com/mcp) で検索や投稿ができます。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに設定されている `github` サーバーを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを確認できました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。

1. `.vscode/mcp.json` を編集し、`github` の隣に `playwright` エントリを追加します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使うため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたときに起動し、信頼するかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して設定を確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認して信頼し、起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて必要としたときに VS Code が起動し、信頼の確認を表示します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを手動で操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、観察内容をまとめます。会話から離れずに機能を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を利用します。ボタン、リンク、リスト項目など、ラベル付きの構造化要素を使ってエージェントが判断するため、簡単な機能テストがアクセシビリティの確認にもなります。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件と照らしてまとめを確認します。動作に問題があれば、追加で質問するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` で動作するアプリが必要です。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したら、チームのレビューに向けてプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミットメッセージを生成します。

   ![変更内容が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

GitHub.com や GitHub CLI からプルリクエストを作成できますが、今回は Copilot に依頼します。関連する Issue を見つけ、プルリクエストに関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使えるように、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトでプルリクエストの作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を取得したり操作を実行したりする際に **Continue** を求められたら選択します。GitHub ツールを初めて使うとき、GitHub 認証を求められる場合があります。指示に従って許可します。
5. Copilot が Issue を検索し、適切なものを見つけてプルリクエストを作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストを確認します。まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は[カスタムエージェント][next-lesson]を作成し、アクセシビリティレビューなど、対象を絞った作業を効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
