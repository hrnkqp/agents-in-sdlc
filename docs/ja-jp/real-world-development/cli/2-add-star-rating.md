---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。各ゲームのデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更を確認して、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始めます。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリ、パブリッシャー、説明しか表示されません。ウォームアップとして、各カードに既存の評価を表示させます。最初のセッションに適した、小さくて独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot アプリとは異なり、通常の CLI 会話では専用の worktree を作らず、現在ターミナルでチェックアウトしているリポジトリと Git ブランチを使用します。保存した会話に戻ることもできます。ファイルやブランチは通常の Git 状態としてディスクに残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するツールの実行状況、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装に向けて新しい会話を始めます。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューします。Copilot CLI で差分を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを調べます。
2. `starRating` に評価がある場合は数値が表示され、値が `null` の場合は `No rating yet` と表示されることを確認します。
3. 両方の状態をテストで確認していることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールの結果は確定的ではないため、実装の詳細は異なる場合があります。特定の実装と照合するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces から転送されたブラウザーで UI を確認します。

1. Copilot に次のように依頼して、アプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能であると通知されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次はコードをプロジェクトにマージするため、プルリクエスト (PR) を作成します。

1. 既定のエージェントに次のように依頼して、変更をコミットします。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI が PR を作成するときに既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更を公開できました。次のことを行いました。

- 機能ブランチで Copilot の会話を始めました。
- ゲームカードへの小さな変更をエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[フィルタリング issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
