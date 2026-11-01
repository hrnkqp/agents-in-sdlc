---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 提出したもの

ワークショップには、更新した `main` からそれぞれ作成した別々のブランチに、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成し、関連テストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、それらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP 経由の Copilot によるブラウザー確認など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した書式で結果を報告しました。QA はこれらの結果と、要件およびテストカバレッジのレビューをまとめて PR の準備状況を確認しました。

追加するテストは、実際に不足しているカバレッジを補うものでなければなりません。追加のテストが不要という QA の結果が正しい場合もあります。変更をレビューして根拠を確認し、コードを変更した場合は関連する根拠を改めて確認してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールによって、作業結果が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は対象を絞った変更に取り組みながら作業状況を把握したいとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

環境と同じくらいコンテキストも重要です。何を作りたいか、なぜ必要か、どのように実装してほしいかを明確に伝えると、出力が大きく変わります。

## さらに確認する

主なワークフローを完了しました。Copilot CLI には、他にも次のような機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot クラウドエージェントに送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。製品コード、趣味のコード、長年考えていた小さなアプリなどで活用してください。チームと学んだことを共有し、お互いの知識を役立てましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、[Copilot クラウドエージェントのワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
