# Manuscript Structure

## 目的

- KDP 向けに、前付・本文・付録・後付を含めた全体構成を固定する
- 執筆用ファイル群と、組版用原稿の並びを対応づける
- 最終整形時に不足要素を見失わないようにする

## 全体構成

1. 表紙
2. 扉
3. はじめに
4. 目次
5. 本文 1章から12章
6. 付録 A から C
7. おわりに
8. 参考情報
9. 著者紹介
10. 奥付

## 組版用の並び

### 前付

1. 扉
2. はじめに
3. 本書の読み方
4. 目次

### 本文

1. 1章 `Hermes Agent とは何か`
2. 2章 `インストールと最初のセットアップ`
3. 3章 `モデル、プロバイダ、ツール権限の考え方`
4. 4章 `CLI を中心とした日常ワークフロー`
5. 5章 `Skills と再利用可能な手順設計`
6. 6章 `MCP 連携で広げる能力`
7. 7章 `Profiles、SOUL.md、Context Files の設計`
8. 8章 `Memory、Memory Providers、文脈の持続`
9. 9章 `Checkpoints と安全な変更`
10. 10章 `Gateway、Cron、長時間運用`
11. 11章 `セキュリティ、承認、運用事故への備え`
12. 12章 `OpenCode / OpenClaw との比較と使い分け`

### 付録

1. 付録 A `ForgeFlow の全体構成`
2. 付録 B `主要機能と責務の対応`
3. 付録 C `代表的な設定断片と安全運用断片`

### 後付

1. おわりに
2. 参考情報
3. 著者紹介
4. 奥付

## ファイル対応

### 本文

- 1章: [01-what-hermes-agent-is.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/01-what-hermes-agent-is.md)
- 2章: [02-install-and-first-setup.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/02-install-and-first-setup.md)
- 3章: [03-models-providers-and-permissions.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/03-models-providers-and-permissions.md)
- 4章: [04-cli-workflows.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/04-cli-workflows.md)
- 5章: [05-skills-and-reusable-routines.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/05-skills-and-reusable-routines.md)
- 6章: [06-mcp-integration.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/06-mcp-integration.md)
- 7章: [07-profiles-soul-and-context-files.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/07-profiles-soul-and-context-files.md)
- 8章: [08-memory-and-context-persistence.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/08-memory-and-context-persistence.md)
- 9章: [09-checkpoints-and-safe-changes.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/09-checkpoints-and-safe-changes.md)
- 10章: [10-gateway-cron-and-long-running-ops.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/10-gateway-cron-and-long-running-ops.md)
- 11章: [11-security-approvals-and-incidents.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/11-security-approvals-and-incidents.md)
- 12章: [12-comparing-hermes-opencode-and-openclaw.md](/Users/goura32/techbook/books/hermes-agent-practical/chapters/12-comparing-hermes-opencode-and-openclaw.md)

### 付録

- 付録 A: [appendix-forgeflow-structure.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-forgeflow-structure.md)
- 付録 B: [appendix-capability-map.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-capability-map.md)
- 付録 C: [appendix-config-and-safety-fragments.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-config-and-safety-fragments.md)

### 制作支援

- 図表計画: [figures.md](/Users/goura32/techbook/books/hermes-agent-practical/figures.md)
- 図表ラフ: [figures-roughs.md](/Users/goura32/techbook/books/hermes-agent-practical/figures-roughs.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/hermes-agent-practical/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/hermes-agent-practical/code-samples.md)
- レイアウト確認: [layout-review.md](/Users/goura32/techbook/books/hermes-agent-practical/layout-review.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/hermes-agent-practical/reference-policy.md)

## 章間のつなぎ

- はじめに:
  - 本書の目的
  - 想定読者
  - `ForgeFlow` を使う理由
  - 読み方の案内
- 本文 1章から4章:
  - 導入と CLI の土台編
- 本文 5章から10章:
  - Skills、MCP、Profiles、Memory、Cron の運用編
- 本文 11章から12章:
  - 安全運用と比較編
- おわりに:
  - 本書全体の視点を短く回収する

## 固定済みの前提

- 扉、前付、後付の構成は固定済み
- 目次は章見出し単位を基本にする
- 参考情報は公式情報優先で転記する
- 発行日は 2026年5月13日を仮設定として保持する

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C の参照表記が本文と一致しているか
- 図表番号と図表キャプションが本文順に並ぶか
- 前付と後付を入れたあとでページバランスが崩れないか

詳細な作業チェックは [final-assembly-checklist.md](/Users/goura32/techbook/books/hermes-agent-practical/final-assembly-checklist.md) を使う。

## 目次粒度の採用方針

- 基本は 1 段階目の見出しのみ
- 章数が多いため、まずは一覧性を優先する
- 節見出しを追加する場合は、長い章だけへ限定する
- 付録と後付の主要見出しは目次へ含める

## 参考情報の最終転記方針

- 公式情報を優先する
- 本文で直接使った判断材料を優先する
- 日付依存の強い項目だけ公開日を併記する
- URL は組版直前に一括で点検して転記する

転記候補の管理は [reference-sources.md](/Users/goura32/techbook/books/hermes-agent-practical/reference-sources.md) を使う。
