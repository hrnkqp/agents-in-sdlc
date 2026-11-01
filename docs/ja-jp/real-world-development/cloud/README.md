---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub 上で作業を割り当てると、クラウドエージェントがバックグラウンドでリポジトリを調べて変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

各演習では、まずクラウドエージェントが従うカスタム指示を追加し、GitHub issue を割り当てて実装を依頼します。カスタムエージェントのレビューと利用、エージェントダッシュボードからのセッションの監視とガイド、プルリクエストのレビューと反復も行います。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | クラウドエージェントが従うカスタム指示を追加します |
| [2. クラウドエージェント][ex2] | 非同期エージェント | issue を Copilot クラウドエージェントに割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視してガイドします |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して次の手順を選びます |

## 前提条件

このワークショップを始める前に、次の準備ができていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランの GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot クラウドエージェント

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI が含まれているため、どのハーネスも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 開始する

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
