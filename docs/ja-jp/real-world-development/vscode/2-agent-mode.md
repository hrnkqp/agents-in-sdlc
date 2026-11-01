---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。必要なファイルを探して変更し、テストを実行して問題を調査し、変更し忘れたファイルを見つけてもう一度修正する、といった作業が続きます。

このような場面で Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構造を調べて必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、Agent mode とクラウドエージェントは異なる用途です。Agent mode は IDE 内で動作し、すばやいフィードバックとやり取りができます。クラウドエージェントはチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数の変更を調整する方法。
- GitHub Copilot Agent Mode が UI とデータレイヤーにわたって機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリと publisher で絞り込めるようにします。前の演習では publisher ヘルパーを追加しました。今回は Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲームクラウドファンディングプラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページはフロントマターから Drizzle ORM 経由でローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習の後で `main` に戻っている Codespace に戻ります。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバーアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、次の主要機能を確認できます。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要に応じて手順で説明します。

Web サイトは動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると issue のバックログも自動で作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューで **Agent** を選択します。Agent は自律的に計画し、複数ファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者は、特定のモデルを推奨しているわけではありません。このワークショップでは Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくするためです。ただし、LLM の出力には確率的な性質があるため、手順と異なる提案が表示されることがあります。これは想定どおりの動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示するコマンドを実行するには **Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトへの機能追加方法を説明する [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使用します。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指針を再利用しやすい小さなファイルに分割し、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルター機能の実装には、少なくとも次の 3 つの更新が必要です。

- データレイヤー (`src/lib/`) の残りのフィルタリングロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

コードをマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. 次のプロンプトを使い、バックログにある関連 issue の要件に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを見つけ、変更とテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更して、テストを実行します。タスクやコードベースによって数分かかる場合があります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。作業が完了すると、すべての変更に対して **Keep** または **Undo** を選べるため、途中で選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat パネルでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のチャットセッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードをレビューすることは常に重要です。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って、Tailspin Toys Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータレイヤーにわたる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して変更を調整する方法。
- コードベースにマージする前に、生成された変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 発展演習 — ページングを実装する

ゲーム一覧が増えると、ページングが必要になります。この演習で学んだことを使い、Copilot にページングを追加するよう依頼します。次の点を検討してください。

- 指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページング方法を検討します。
- 希望する実装に必要な情報を Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを追加して何度かやり取りする必要があるかもしれません。Copilot と作業する場合の通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の Agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
