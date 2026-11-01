---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人がレビューできるドラフト PR を作成します。面倒な作業を委任して開発を加速し、開発者がより大きな課題に集中できるようになります。

Copilot クラウドエージェントを使って、次のことを確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- スコープを明確にした issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを残さなかったため、まずはアプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリの別のゲームを表示し、閲覧を続けられるようにします。現時点では、ユーザー受け入れテストに使える簡単な実装が必要です。これは現在のブロッカーですが、さらに優先度の高い issue もあります。

どちらも、優先度が下がりやすいタスクの例です。Copilot クラウドエージェントに割り当てれば、開発者は別の作業を進めながら、後で Copilot の作業をレビューできます。

## GitHub Copilot クラウドエージェントの紹介

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行します。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などで作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境を設定して、タスクに取り組みます。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストであなたに通知します。

## スコープを明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な作業でも、詳しく調べると複雑なことがあります。[Copilot クラウドエージェントに依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整しましょう。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその役割を確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、このタイミングで一度確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認すると、後から指示の影響を確認できます。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する指示を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、`**/*.test.ts` ファイルに指示が適用されることを確認します。

次のセクションで **Code lacks documentation** issue をクラウドエージェントに割り当てる際、これらの指示に基づく TSDoc コメント、TypeScript の規約、コメントヘッダーが PR に反映されているか確認してください。後の演習で PR をレビューする際にも取り上げます。

## Copilot クラウドエージェントの開発環境を設定する

コードの作成には、通常、適切な状態を確保するための環境やセットアップスクリプトが必要です。SWE と同様に作業する Copilot にタスクを割り当てる場合も同じです。

クラウドエージェントは GitHub Actions を使って作業環境を用意します。作業開始前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して環境をカスタマイズすると、必要な開発ツールや依存関係を使用できるようになります。このラボでは、学習を進めやすくするため、すでに設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザーバイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は自身の操作用トークンを受け取ります
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ — Node.js (アプリは Astro + Drizzle/libSQL)
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

      # ビルドとテスト用にローカル SQLite データベースをマイグレーションし、シードします
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同じ構成ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあります。Copilot がプルリクエストの作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、多くのプロジェクトで情報が古かったり、ドキュメントがなかったりします。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメント作成が得意なため、クラウドエージェントに割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成し、後の演習でその作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは **[WIP]: Code lacks documentation** のようになります。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成したことを確認できます。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで、数分かかる場合があります。Copilot が環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. TODO リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、適切な計画を考え、コードを作成するまでの動きを確認できます。

この作業には数分かかる場合があります。Copilot クラウドエージェントの主な目的は、非同期でタスクを実行し、開発者が別の作業に集中できるようにすることです。次は別のタスクもクラウドエージェントに割り当て、アプリケーションに機能を追加するコードを作成します。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot クラウドエージェントの利点の 1 つは、作業を分担できることです。デザインチーム向けの関連ゲーム機能は時間がかからないかもしれませんが、その時間を別の作業に使えます。クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が issue の最初のコメントに表示されます。

![issue で Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot クラウドエージェントは SWE と同じように動作するため、常に監視する必要はありません。作業が終わってからレビューできます。次はカスタムエージェントの作成と使用を確認しましょう。

## まとめと次のステップ

[GitHub Copilot クラウドエージェント][copilot-agents]について学びました。Copilot に issue を割り当てると、非同期にタスクを実行できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次のことを確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- スコープを明確にした issue の重要性。
- issue を Copilot に割り当てる方法。

クラウドエージェントがバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に取り組みます。[Copilot クラウドエージェントは MCP サーバー][cloud-agent-mcp]と、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

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
