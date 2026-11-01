---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションです。エージェント主導の開発に必要な機能を 1 つのワークスペースにまとめています。並列エージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub issue とプルリクエストのネイティブな管理機能を提供します。**Agent Merge** を使うと、プルリクエストのリベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージを管理できます。

このワークショップでは、Tailspin Toys で一連の作業を続けて行います。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリング issue から始め、**Plan** モードで方針を決め、**Autopilot** モードで実装して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルタリング機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリング機能を確認します。
7. 品質保証 (QA) のカスタムエージェントを作成して、要件、テストカバレッジ、検証の根拠をレビューします。
8. フィルタリング機能の変更全体を確認し、2 つ目の PR に Agent Merge を使います。
9. キャンバスが利用可能か確認し、Database Explorer を使います。その後、リポジトリと連携するトリアージキャンバスを作成してテストします。必要な機能が利用できない場合は、その阻害要因を記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップを集中して進めるため、PR は 2 つ作成します。最初は星評価、次は指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングです。それぞれ更新済みの `main` から開始します。フィルタリングと品質ワークフローでは、1 つのセッション、worktree、ブランチを使い、各ツールを学びながら作業を積み重ねます。最後のキャンバス演習では同じセッションを使い、PR ワークフローを繰り返さずに、共有画面の作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app をインストールする][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue に基づいて機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルタリング機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変えて実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザー確認 | Customize から MCP を設定し、フィルタリングの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選択し、最終的な検証根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、指示、スキル、QA プロファイル、テストを確認し、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリと連携するトリアージキャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランが設定された GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、各ハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。これは Copilot CLI のポリシーとは別の設定です。

## 開始する

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
