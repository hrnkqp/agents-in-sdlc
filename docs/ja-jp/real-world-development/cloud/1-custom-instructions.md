---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot クラウドエージェントは GitHub issue に対して非同期で動作するため、リアルタイムの比較が難しいためです。クラウドエージェントが作成するプルリクエストをこのハーネスの後半でレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。規約、ライブラリ、好みのコメント形式などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指針を適用するためのファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクに合わせて作成できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準形式の指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することはこのワークショップの範囲外ですが、サンプルプロジェクトでは代表的な方法を紹介しています。基本として、次の点を意識してください。

- `copilot-instructions.md` には、構築するもの、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなど、特定のファイル形式やタスクに関する指針を記載します。
- 自然な言葉で、明確な指示とコード例を使います。

指示ファイルの作り方や AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意すると役立ちます。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。画面ごとに手順が異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。該当する画面のレッスンで手順を説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決めるリポジトリルート相対の glob です。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. ユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さな再利用可能なファイルに分割し、コード生成の参考例を Copilot に示せます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

次に行う編集は、コード内のドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーで記述するよう、プロジェクト全体のルールを追加することです。後続の演習で Copilot クラウドエージェントが issue に取り組む際、このルールを参照します。

編集する前に、作業用のブランチを作成します。クラウドエージェントは issue が対象とするブランチの指示を読みますが、変更を整理してコミットするため、ここではブランチを作成します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるためのファイルです。コード提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはありません。このため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 次の Markdown を既存の標準の直後に追加し、ファイルコメントヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、リポジトリレベルの `.github/copilot-instructions.md` は全体的な指針に、`*.instructions.md` は特定の言語、ファイル形式、タスクの指針に適しています。今回追加したドキュメントコメントのルールのような、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは issue の対象ブランチから作業を開始します。次の演習で issue を Copilot に割り当てると、`main` からブランチが作成されます。そのため、クラウドエージェントがこの指示を参照できるよう、変更を `main` に反映してください。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue を割り当てると、クラウドエージェントはこの指示を参照します。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して、`main` に直接コミットできます。ここでは、実際のプロジェクトでの手順に近づけるためにブランチを使用しています。

## まとめと次のステップ

このプロジェクトの指示ファイルで Copilot にコンテキストを伝える方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの残りでクラウドエージェントが作成するプルリクエストをレビューする際に、この標準が使われていることを確認できます。

次に、[Copilot クラウドエージェントに最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
