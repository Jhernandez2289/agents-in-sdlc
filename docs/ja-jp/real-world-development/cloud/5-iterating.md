---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加に関する複数の作業で GitHub Copilot を使いました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。変更を確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チームメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督者なしで非同期に作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限定されます。
- cloud agent は GitHub Actions 内に一時的な独立環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。外部リソース (外部 MCP サーバーへの呼び出しなど) へのアクセスは既定で制限されているため、これは**想定される動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントなど、更新されたコードを確認します。実際の変更は異なる場合があります。Exercise 2 の冒頭で確認したカスタム指示に基づく TSDoc コメント、TypeScript の規約、ファイルヘッダーに注目してください。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーに依頼する場合と同様に、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確定的ではないため、依頼内容を一律に指定することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. コードベースに追加したいドキュメントの変更をコメントで依頼し、他のユーザーと同様に **@copilot** をメンションします。上記の例を使うか、別の変更を依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新用の新しいセッションが始まります。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わったら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![ドラフト PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を探します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、PR の **Conversation** タブに新しいコメントを追加し、Copilot にメンションして変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、カスタム指示の更新前に割り当てた作業にコメントヘッダーや TSDoc コメントを追加したりできます。Copilot はコメントを確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot が作業中の場合は PR のタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 次の点に注意して実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイコントラストとライトモードの CSS またはスタイルの変更
   - ARIA ラベル、キーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装をガイドしたことを確認します。改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 作業の委任を続ける

cloud agent には実際のバックログ作業を任せ、その間に別の作業をするのが効果的です。リポジトリに追加の Issue を登録し、Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心表明フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。

- リポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に反映されることを確認しました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ向けのカスタムエージェントを作成して使用し、ハイコントラストとライトモードのトグルを追加しました。
- Copilot agents ページをミッションコントロールとして使い、進行中のアクセシビリティセッションを監視して方向付けました。
- Copilot の PR をレビューして反復し、**@copilot** に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。ここで試した内容をさらに広げるには、次のハーネスを確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、レビュー済みの 2 つの変更をターミナルから公開します。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野に特化したカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
