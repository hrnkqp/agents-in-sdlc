---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue に基づくフィルタリング機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織の既存のワークフローに沿って適切なものを適切な方法で構築できるよう支援します。このレッスンから、issue に基づいて機能を作成し、コードが正しく、期待どおりに動作することを確かめ、最終的にプロジェクトにマージする、エージェントを活用した開発プロセスを進めます。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常、異なる種類のファイルごとにブランチや PR を分けますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリとパブリッシャーでゲームを絞り込みたいと考えています。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan モードで決定し、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発に取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 手順の詳細はチームや組織によって異なりますが、ほとんどは上記の流れに沿っています。

標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビューと検証を経るようにできます。

## 会話モード

**会話モード**は、エージェントがどれだけ自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次の操作に進む前に入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から作業を始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、事前に計画することをお勧めします。Plan モードに切り替えて計画の作成を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を判断して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画全体を表示します。
5. 計画を上下にスクロールして確認します。
6. 希望と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできました。Autopilot を使って実装します。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンに表示される同様の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業を繰り返し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。最初に計画を確認してください。依存関係が不足している、ポートが競合しているなどの報告があれば、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを調べます。
3. 結果を issue と計画中に決めた内容と比較します。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を確認する

コードに問題がなさそうなら、Codespaces の転送ポートでサイトを開き、実際に動くか確認しましょう。

1. Copilot に次のように依頼して、アプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能であると通知されたら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリフィルター、パブリッシャーフィルター、その組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と合意内容に一致することを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示を使って][next-lesson]コード生成の指針を確認し、文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
