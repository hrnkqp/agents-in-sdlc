---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを使って機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot へのツール追加方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外部に接続できると、さらに便利になります。Copilot がそれを実現する方法が Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![Agent mode の内部と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) とのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。最新情報 (リソース) にアクセスしたり、ユーザーに代わって操作 (ツール) を実行したりできます。

ツールやリソースは MCP サーバー経由で利用します。MCP サーバーは AI エージェントと外部のツールやサービス (既存の API や NPM パッケージなどのローカルツール) をつなぎます。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。リポジトリの作成や更新、issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

さまざまなツールやリソースを提供する MCP サーバーが他にもあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくして貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、他の依存関係と同様に扱ってください。使用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot は issue のバックログを確認できました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに使用できます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると画面を表示せずにブラウザーを実行できます。Codespace には表示用デスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストで Playwright を使うため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼し、再度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを必要としたときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認し、サーバーが起動できるように信頼します。

手動での起動は省略できます。その場合、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを開き、ページを移動してフィルターを適用し、結果を読み取って報告できます。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ラベル付きの構造化された要素 (ボタン、リンク、リスト項目など) を認識します。そのため、簡単な機能確認をアクセシビリティの簡易チェックにも活用できます。

サーバーを接続し、アプリを実行したら、次のプロンプトを Copilot に送信してフィルター機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、手順に沿って操作して結果を報告します。issue の受け入れ条件と照らして報告を読み、問題があれば質問したり、プルリクエストを作成する前にコードを修正するよう依頼したりします。

> [!NOTE]
> このテストには、`http://localhost:4321` で実行中のアプリが必要です。開発サーバーを停止している場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使用する際に、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームでレビューするために PR を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が加えた変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、リポジトリにブランチをプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使って、関連する issue を見つけ、関連付けた PR を作成してもらいましょう。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使用できるよう、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のように Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めて操作を実行する際、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは認証を求められることがあります。表示される手順に従ってください。
5. Copilot が issue を検索し、該当する issue を見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストするために使用しました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を「テスト」しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタムエージェントを作成][next-lesson]し、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
