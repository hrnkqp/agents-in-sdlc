---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめて確認し、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質ワークフロー全体を同じ会話とブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、プルリクエストのマージプロセスを自動化する方法を学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能を実装する過程で、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、マージを許可する権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするための残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正したり、レビューコメントに対応したり、必要に応じてリベースしたりして、PR の阻害要因を解消します。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体を確認して PR を作成し、残りの処理を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを入力して PR を作成します。
5. `/pr agentmerge` を入力して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. Copilot から **PR #14 was squash-merged successfully.** のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発の複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージプロセスを自動化する方法を学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は便利な Copilot CLI のスラッシュコマンドを[確認][next-lesson]し、コンテキスト、モデル、共有、任意のクラウド委任を試します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
