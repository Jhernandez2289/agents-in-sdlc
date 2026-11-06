---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法の規則や Copilot が知っておくべき背景情報がある場合は、それを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルから、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは指示を追加する前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue で非同期に作業するため、リアルタイムに並べて効果を示すのが難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードを基本とし、現代的な見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの開発規約に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコード生成を促せます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られます。技術スタック、開発中の内容、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の作業やファイル種別に関するガイドラインを記載するファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といった作業を対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトの例を参考に、次のように使い分けます。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体のコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別や作業に関する指示を記載します。
- 自然な言葉で明確に記述し、コードのよい例と避けるべき例を示します。

指示ファイルの作成方法は 1 つではありません。プロジェクトに合う方法を試しながら見つけてください。

> [!TIP]
> このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けの指示ファイルがあります。
>
> Copilot を使って指示ファイルを生成することもできます。操作方法は利用する環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要な場合は、利用中の環境向けレッスンで説明します。
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

## リポジトリ全体に適用する規約を追加する

ここで行う編集は 1 つです。`.github/copilot-instructions.md` にプロジェクト全体の規則を追加し、cloud agent が後の演習で Issue に取り組むときに使えるようにします。

編集前に作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更をコミットするためにもブランチを使います。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にリポジトリのコーディング規約を記載し、コード提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング規約は記載されていますが、コード内のドキュメントに関する規則はまだありません。
3. 既存の規約の直後に、次の Markdown 行を追加し、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語やファイル種別、作業に限定した指示は `*.instructions.md` に記載できます。今回追加したドキュメントコメントの規則はプロジェクト全体に適用するため、リポジトリレベルのファイルが適切です。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は Issue が対象とするブランチから指示ファイルを読み取ります。次の演習で Issue を割り当てると、Copilot は `main` からブランチを作成します。指示を適用するには、この変更を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で cloud agent に Issue を割り当てると、更新した指示が使われます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの手順を省略して `main` にコミットできます。ここでは実際のプロジェクトに近い手順を使っています。

## まとめと次のステップ

指示ファイルから Copilot にコンテキストが伝わる方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。この規約は、このハーネスの後半で cloud agent が作成するプルリクエストに反映されます。

次は[最初の Issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
