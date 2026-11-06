---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法の規則や Copilot が知っておくべき背景情報がある場合は、それを参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成に関するガイダンスを Copilot に提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルから、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- 現在の指示を適用した状態で、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい規約に従う様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約から外れる場合があります。Copilot の動作は確定的ではありません。この演習では出力を一字一句一致させるのではなく、指示の更新後に傾向がどう変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードを基本とし、現代的な見た目にします。
- コードには TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの開発規約に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコード生成を促せます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の作業やファイル種別に関するガイドラインを記載するファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といった作業を対象にできます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

サンプルプロジェクトの指示ファイルを参考に、次のように使い分けます。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体のコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別や作業に関する指示を記載します。
- 自然な言葉で明確に記述し、コードのよい例と避けるべき例を示します。

指示ファイルの作成方法は 1 つではありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けの指示ファイルがあります。
>
> Copilot を使って指示ファイルを生成することもできます。操作方法は利用する環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要な場合は、各環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、基本となる `copilot-instructions.md` と、さまざまな作業向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示も確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドはリポジトリのルートからの glob を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、コード生成の例を Copilot に示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の作業を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. Copilot に必要なコンテキストだけを与えるため、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認してコードを更新します。ヘルパーファイルとテストが変更されることもあります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが目的です。

## リポジトリ全体に適用する規約を追加する

`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コーディング規約を記載し、コード提案を改善します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング規約は記載されていますが、コード内のドキュメントに関する規則はまだありません。
3. 既存の規約の直後に、次の Markdown 行を追加し、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語やファイル種別、作業に限定した指示は `*.instructions.md` に記載できます。今回追加したドキュメントコメントの規則はプロジェクト全体に適用するため、リポジトリレベルのファイルが適切です。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再び開き、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約を Copilot に伝え、次の演習で使う実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのリポジトリアセットと同じように、ソース管理で管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 次のコマンドを実行し、指示と Copilot がヘルパー用に変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けたプルリクエストを作成します。GitHub ツールがプロジェクトに接続されているので、Copilot に依頼できます。

> [!NOTE]
> Copilot に GitHub の操作を依頼するのはこれが初めてです。プロジェクトのテンプレートには GitHub ツールがすでに設定されています。Copilot が最初にツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。指示に従って許可してください。接続に使われる Model Context Protocol (MCP) については、後の演習で説明します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチからプルリクエストを作成するよう Copilot に依頼します。プルリクエストには指示の更新と、今後のフィルター機能で使う publishers ヘルパーの基盤を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作の許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
5. プルリクエストの作成後、Copilot にマージを依頼し、ローカルのブランチを `main` に戻して最新コードを取得します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作やシェルコマンドの実行許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージしました。

## まとめと次のステップ

指示ファイルから Copilot にコンテキストが伝わる方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示でコード生成プロンプトを実行し、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。
- 同じプロンプトを再度実行し、提案コードが新しい規約に従う様子を確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は [エージェントモード][next-lesson]で指示を使い、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
