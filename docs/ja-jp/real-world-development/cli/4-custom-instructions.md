---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリング機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行したい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、希望するコードの「何を」だけでなく、「どのように」も説明できます。フィルタリング機能を実装できたので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの繰り返しではなく、意図や自明ではない判断を記載します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入する場合は、その説明も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたときにコメントも最新の状態にします。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。規約、ライブラリ、好みのコメント形式などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られます。ほとんどのリクエストに関連するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに指針を適用します。

> [!NOTE]
> 他の指示形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに複数の指示ファイルがあります。変更の影響を確認する前に、その内容を調べましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが適用対象ファイルを決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクトのファイルへのリンクを確認します。指示を小さなファイルに分け、再利用しやすくできます。

## チームの方針に合わせて指示ファイルを更新する

既存の指示ファイルは出発点として十分ですが、TypeScript に新しいコードを生成するときに TSDoc コメントを追加するルールがありません。中核となる `copilot-instructions.md` を更新しましょう。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリングのコンテキストを保ったまま、会話を終了して再開すると、新しい指示を読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリの `game-filters-cli` ブランチから、保存された会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明がコードと一致していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取り込む方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
