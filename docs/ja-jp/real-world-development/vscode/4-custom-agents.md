---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成して、対象を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。最も分かりやすい違いは、**カスタムエージェントが作業者、スキルがツール**であることです。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一環としてスキルやほかのエージェントを編成できます。このラボのアクセシビリティ用カスタムエージェントは、サイトをアクセシビリティガイドラインに照らしてレビューし、更新します。その作業の一環として、プルリクエストワークフローやテスト実行・管理のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダークテーマを読みづらいと感じるユーザーがいます。そこでデザインチームは、切り替え可能なハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに作成されています。Copilot をガイドする内容を確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ機能用の新しいブランチを `main` から作成します。演習 3 のフィルタリング PR とは別に作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択メニューには、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選ぶと、そのエージェントの指示とツールに対象を絞ったチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要に応じて手順で説明します。

1. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント選択メニューを選択します。現在のエージェント (既定では **default**) が表示されています。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトのファイルに適用します。変更ごとにファイルパスと差分がチャットに表示されます。
6. **Files changed** の表示が更新され、このセッション中にエージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを変更します。

次の演習では、作業中の内容を確認し、必要に応じてエージェントをガイドします。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に合わせたエージェントを使うと、チームの専門知識や標準を再利用可能な形でまとめ、Copilot により適切な作業を促せます。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat の Agent mode でカスタムエージェントを使う方法。

次は[エージェントの作業を監視してガイド][next-lesson]します。変更を確認し、同じセッションにライトモードの切り替えも追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
