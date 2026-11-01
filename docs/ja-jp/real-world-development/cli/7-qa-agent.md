---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリング操作を確認しました。次はそれらをカスタムエージェントにまとめ、明確な QA プロセスを定義します。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリング issue の要件に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。プロファイルにはエージェントの目的、指示、使用可能なツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告するよう Copilot に指示します。それらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実施するチェックと守るべき境界の両方を記載します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと境界を反映したことを確認します。確認だけを依頼しているのに、エージェントが範囲の広い変更を加えないようにするためです。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定するとエージェント名が分かりやすく表示されます。
3. プロファイルの指示を読み、QA が要件から作業を始め、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使用することを確認します。
4. QA が根拠を報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルタリング会話を再開し、QA を選択します。そうすると、すでに会話にある issue と計画の決定を利用できます。

1. 次のプロンプトを使って QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進めます。
4. 作業完了後に表示されるレポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントをワークフローに加え、その作業を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成し、内容を確認しました。
- QA エージェントを選択し、issue の要件に照らして機能のレビューを依頼しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次はこれらを機能 PR にまとめ、Agent Merge を使います。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
