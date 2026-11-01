---
title: "演習 3 - カスタムエージェント"
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

この演習では、次のことを行います。

- カスタムエージェントの定義方法を確認します。
- カスタムエージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダークテーマを読みづらいと感じるユーザーがいます。そこでデザインチームは、切り替え可能なハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をガイドする内容を確認しましょう。

Codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## issue を作成して割り当てる

ミッションコントロールは、環境内のエージェントを操作するための中心的な場所です。Copilot クラウドエージェントにタスクを割り当てたり、進行状況を監視したり、進行中のセッションをガイドしたりできます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの割り当て画面。Accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に合わせたエージェントを使うと、チームの専門知識や標準を再利用可能な形でまとめ、Copilot により適切な作業を促せます。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方法。

Copilot がハイコントラストモードを実装している間に、次はミッションコントロールから[エージェントセッションを監視してガイド][next-lesson]します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
