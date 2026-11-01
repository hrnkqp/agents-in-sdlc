---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを使いました。次は**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェントセッションを開始し、セッションの構造を学びます。
- プロジェクトに小さく、範囲を絞った変更を加えるようエージェントに依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリ、publisher、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。小さくて独立した変更なので、最初のセッションに適しています。

## セッションの構成

**セッション**は、エージェントとの会話を独立したワークスペースで実行するものです。各セッションには**専用の Git worktree とブランチ**が割り当てられます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにサイドバーにまとめて表示されます。セッションを選択すると切り替えられます。

セッション内では、エージェントとの**会話**、ファイルを調査・編集する際の**ツールの実行状況**、差分とともに表示される**変更ファイル**の一覧を確認できます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを調べて機能を実装します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して、変更を依頼しましょう。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選択します。次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、最初にプロジェクトの分離されたコピーである新しい worktree を作成します。続いてプロジェクトを調べ、新機能に必要なファイルを見つけてコードを作成します。Copilot app で新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更ファイルと差分を確認します。

    ![Create PR ボタンの右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細表示に使用される中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" に切り替える、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールの結果は確率的なため、実際のコードは上記と異なる場合があります。大まかには同じ動作になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示がテストされていることを確認してください。前提条件が不足していたり、チェックがスキップされたりしている場合、成功とは見なせません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動くと判断するのではなく、Copilot に Web サイトを開いてもらい、更新した UI を確認します。サイトの起動を依頼して、ブラウザーキャンバスで開きましょう。

> [!TIP]
> キャンバスは Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べて自分でも作成します。ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに 5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使って、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次はプルリクエスト (PR) を作成し、新しいコードをコードベースにマージします。

1. 右上隅にある **Create PR** を選択します。
2. 表示された場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR 作成前に関連する issue が必要だと Copilot が説明した場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択して、レビューペインで PR を開きます。ここで内容を確認できます。
6. 問題がなければ **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って、最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェントセッションを開始し、セッションの構造を学びました。
- ゲームカードに小さく、範囲を絞った変更を加えるようエージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、自分でマージしました。

次は[フィルタリング issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
