---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

このラボでは、ユーザーエクスペリエンスの改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、関連ゲーム機能の実装、ハイコントラストおよびライトモードの切り替えなどを依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送信しましょう。

## 作業をレビューする

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードを確認し、通常の DevOps プロセスに従います。チームでコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## GitHub Copilot クラウドエージェントのセキュリティ

Copilot クラウドエージェントは非同期に、監督者なしでタスクを実行するため、安全性を維持するための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みできるのはコード用ブランチだけです。
- クラウドエージェントは GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot クラウドエージェントが作成した、コードへのドキュメント追加に関するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは想定どおりの動作です。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザータブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択し、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が付いています。その場合は作業が完了するまで待ちます。数分かかることがあります。休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを確認します。実際の変更は環境によって異なります。

    変更を確認しながら、演習 2 の最初に確認したカスタム指示ファイルから反映された TSDoc コメント、TypeScript 規約、コメントヘッダーを探します。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows を選択するボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエスト上でやり取りすることもできます。チームメンバーと同じように、プルリクエストのコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は予測できないため、必ずこう依頼すべきという文面はありません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、簡単な説明を記載したコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使って、生成されたドキュメントの変更を依頼するコメントを追加し、通常のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択するとプルリクエストに戻れます。

    ![Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで作業を続けてください。完了したら、ドラフトの PR をレビュー可能な状態に変更して、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更するボタン](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows を選択するボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。以前と同じように、Copilot クラウドエージェントに更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます。後者はカスタム指示を変更する前に割り当てたタスクなので、追加を依頼する必要があるかもしれません。ほかの PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が付いています。その場合は完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューします。特に次の点を確認してください。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するローカルストレージの使用方法。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. 承認待ちのワークフローがある場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows を選択するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタムエージェントが、Copilot をアクセシビリティのベストプラクティスに沿ってガイドしたことを確認しましょう。アクセシビリティに関する懸念や改善点がある場合は、前の PR と同じように、コメントで **@copilot** をメンションして更新を依頼できます。

## 任意の演習 — さらに作業を委任する

クラウドエージェントには、実際のバックログ項目を割り当てて別の作業に取り組む方法が適しています。この習慣を身につけるために、リポジトリにいくつか issue を作成し、Copilot に割り当ててみましょう。次のような作業が考えられます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントのハーネスを完了しました。各レッスンで、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、クラウドエージェントの出力への影響を確認しました。
- issue を Copilot クラウドエージェントに割り当て、環境の設定、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使用し、ハイコントラストモードとライトモードの切り替えを追加しました。
- Copilot エージェントページをミッションコントロールとして使用し、処理中のアクセシビリティセッションを監視してガイドしました。
- クラウドエージェントのプルリクエストをレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

クラウドエージェントのハーネスはこれで完了です。学んだことを他のハーネスでも活用できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の Agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、クラウドエージェントに戻る任意の `/delegate` ブリッジを使い、レビュー済みの変更を 2 つターミナルから作成します。

自分のリポジトリでは、次の作業を試してください。

- リファクタリングまたはテストカバレッジの issue をクラウドエージェントに割り当てます。
- 別の分野に対応するカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
