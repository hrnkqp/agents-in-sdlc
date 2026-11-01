---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことが開発のすべてではありません。手動でコードの動作を確認し、指示ファイルでコーディング標準に従うようにしました。テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、Copilot にテストや lint などの実行方法を伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、読みやすいレポートにするため、報告形式を改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、エージェントが必要に応じて読み込む再利用可能なタスク指示、実行可能なスクリプト、補助リソースが含まれます。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルを定義する `name` と `description` を含むフロントマター、機能の概要、呼び出すべきタイミングの指針を記載します。必要に応じて、スキルが使うスクリプトやその他のリソースをサブフォルダーに追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルが必要とは限りません。Tailspin Toys の `quality-checks` スキルには `SKILL.md` だけがあり、プロジェクトにあるコマンドを使います。

スキルはプロジェクトの `.github/skills` フォルダーに配置してチームで共有、再利用できるようにするか、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明を基に、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の流れをどのようにガイドしているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出したり、自然言語で依頼したりできます。スキルに 3 つのチェックを実行してもらいましょう。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したのか、成功したか、ツールが実際に何を報告したのかを分かりやすく示すレポートにしましょう。スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 変更は自動的に保存されます。

## 更新したスキルを実行する

更新した内容が反映されるか確認しましょう。Copilot CLI では、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果レポートの形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルタリング機能とともに機能 PR に含まれます。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## その他のスキル例

コミュニティの以下の例は参考資料であり、追加タスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
