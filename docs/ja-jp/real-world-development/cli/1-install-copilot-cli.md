---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、登録済みのフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを使えます。コマンドラインから作業を任せたり、変更を依頼したりできるため、作業の流れを保てます。まずは、使い慣れたツールで Copilot CLI をインストールしましょう。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーでフィルタリング issue を見つけます。

## シナリオ

チームは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI はその機能をターミナルに取り込みます。ターミナルは多くの開発者が日常的に作業する場所です。このレッスンでは、CLI をインストールして認証し、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js が事前にインストールされているため、ここでは npm を使用します。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上と表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認して、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みバージョンのコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正常に動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルタリング issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue を読みます。最初の小さな変更を終えた後に、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使用し、自動的に worktree を作成しません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は設定済みの権限外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。ユーザーが承認したこのショートカットを Codespace 内で使うと、承認のプロンプトが表示されず、演習に集中できます。

> [!CAUTION]
> `--yolo` はすべての操作に対する自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` は、後のレッスンで issue やプルリクエストを操作するために使う GitHub MCP の読み取り・書き込みツールを有効にします。Codespace によりローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには実際にアクセスできます。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて別の簡単な質問をして、会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使うと、同じ機能会話とブランチに戻れます。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルタリング issue を見つけました。

次は[最初の小規模な変更を始め][next-lesson]、Copilot CLI にゲームカードの星評価を表示させます。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
