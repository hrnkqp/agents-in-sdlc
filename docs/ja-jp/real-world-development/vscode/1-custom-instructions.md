---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を伝える方法がいくつかあります。重要な方法の 1 つが[指示ファイル][instruction-files]です。指示ファイルでは、コード生成の「何を」だけでなく、「どのように」も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の指示を使って、フィルター機能の最初のデータスライス (publisher ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 指示の変更とフィルタリング機能を `main` にコミットし、次の演習で Copilot が最新の指針を利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の結果は確定的ではありません。この演習の目的は、指示の変更後に挙動がどのように変化するか確認することであり、出力を一字一句一致させることではありません。

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

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られるファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、プロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに指針を適用します。たとえば、TypeScript、UI コンポーネント、ユニットテストの作成などです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示と `applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準形式の指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

サンプルプロジェクトには、指示ファイルの代表的な構成が含まれています。基本として、次の点を意識してください。

- `copilot-instructions.md` には、構築するもの、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルの指針を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなど、特定のファイル形式やタスクの指針を記載します。
- 自然な言葉で、明確な指示とコード例を使います。

指示ファイルの作り方や AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意すると役立ちます。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用するなど、画面ごとに手順が異なります。該当するレッスンで手順を説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このプロジェクトに含まれる指示ファイルを確認し、変更による影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドが適用対象ファイルを決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクトの既存ファイルへのリンクを確認します。大きな指示を再利用しやすい小さなファイルに分割し、参考例を示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイルコメントヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用ブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要に応じて手順で説明します。

カスタム指示の効果を確認するため、最初に現在の指示ファイルのままでプロンプトを送信します。後で指示を更新して、同じプロンプトをもう一度送信します。

1. Copilot に必要なコンテキストだけを与えるため、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューで **Agent** を選択し、Copilot がファイルを編集できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーファイルとテストを含むコード変更を適用します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot の結果は確率的なため、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。指示を追加した後に挙動が一貫することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を伝えるためのファイルです。コード提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントのルールがありません。
3. 既存の標準の直後に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` は全体的な指針に、`*.instructions.md` は特定の言語、ファイル形式、タスクの指針に適しています。今回追加したドキュメントコメントのルールのような、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を開き、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習の基礎となる実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他の資産と同じようにバージョン管理します。変更をコミットして、リポジトリにブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更を確認します。
3. 指示の更新と、ヘルパーの基礎実装のために Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けたプルリクエストを作成します。GitHub Copilot に issue を探させ、issue をクローズする PR を作成してマージさせます。

> [!NOTE]
> この演習では、Copilot が GitHub に対して操作を行います。プロジェクトのテンプレートには GitHub ツールがすでに接続されています。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。表示される手順に従ってください。後の演習で、この接続に使われる Model Context Protocol について確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** を選択します。
3. 指示ファイルの更新と publisher ヘルパーの追加の両方を説明する、現在のブランチからの PR を作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub の操作を実行する許可を求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトを使ってマージし、ローカルブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作とシェルコマンドの許可を求められたら、内容を確認して適切な場合に **Allow** を選択します。

これで GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルで Copilot にコンテキストを伝える方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の変更とヘルパーの基礎実装を `main` にコミットし、次の演習で使えるようにしました。

次は [Agent mode][next-lesson]を使って、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
