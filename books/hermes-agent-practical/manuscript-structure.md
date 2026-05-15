# Manuscript Structure

## 目的

- KDP 向けに、前付・本文・付録・後付を含めた全体構成を固定する
- 執筆用ファイル群と、組版用原稿の並びを対応づける
- 構成刷新後も、本文と付録の役割分担を崩さないようにする

## 全体構成

1. 表紙
2. 扉
3. はじめに
4. 本書の読み方
5. 目次
6. 本文 1章から12章
7. 付録 A から H
8. おわりに
9. 参考情報
10. 著者紹介
11. 奥付

## 組版用の並び

### 前付

1. 扉
2. はじめに
3. 本書の読み方
4. 目次

### 本文

1. `Hermes Agent をどう捉えるか`
2. `インストール、ディレクトリ構造、最初の設定`
3. `モデル、プロバイダ、権限設計`
4. `CLI 中心の日常ワークフロー`
5. `Skills と再利用可能な作業手順`
6. `MCP 連携の設計と運用`
7. `Profiles、SOUL.md、Context Files`
8. `Memory と文脈の持続`
9. `Checkpoints、変更管理、ロールバック`
10. `Gateway、Cron、常駐運用`
11. `セキュリティ、承認、事故対応`
12. `OpenCode / OpenClaw と比較した採用判断`

### 付録

1. `ForgeFlow の全体構成`
2. `主要機能と責務の対応`
3. `設定断片と安全運用断片`
4. `設定キー完全表 1`
5. `設定キー完全表 2`
6. `プラットフォーム・安全性・運用キー完全表`
7. `互換キー・動的キー完全表`
8. `認証系環境変数とプロセス上書き一覧`

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
- 付録 D: [appendix-configuration-reference.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-configuration-reference.md)
- 付録 E: [appendix-environment-variable-catalog.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-environment-variable-catalog.md)
- 付録 F: [appendix-platform-and-ops-reference.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-platform-and-ops-reference.md)
- 付録 G: [appendix-compatibility-and-dynamic-reference.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-compatibility-and-dynamic-reference.md)
- 付録 H: [appendix-process-overrides.md](/Users/goura32/techbook/books/hermes-agent-practical/appendix-process-overrides.md)

### 制作支援

- 図表計画: [figures.md](/Users/goura32/techbook/books/hermes-agent-practical/figures.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/hermes-agent-practical/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/hermes-agent-practical/code-samples.md)
- レイアウト確認: [layout-review.md](/Users/goura32/techbook/books/hermes-agent-practical/layout-review.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/hermes-agent-practical/reference-policy.md)

## 本文と付録の役割分担

- 本文:
  - 判断軸
  - 代表的な設定例
  - 失敗例と切り分け
  - どこまで任せ、どこで止めるか
- 付録:
  - 通しサンプル全体図
  - 機能ごとの責務整理
  - 最小構成の断片
  - 設定項目ごとの既定値、関連 env、関連 CLI を引ける完全表
  - 互換キー、動的キー、認証系 env、プロセス単位上書きの補助資料

## 章間のつなぎ

- 前付:
  - 本書の目的
  - 想定読者
  - `ForgeFlow` を使う理由
  - 付録の使い分け
- 本文 1章から4章:
  - 導入と運用土台編
- 本文 5章から9章:
  - 能力追加と状態管理編
- 本文 10章から11章:
  - 常駐運用と安全設計編
- 本文 12章:
  - 採用判断編
- 後付:
  - 参考情報と著者情報

## 固定済みの前提

- 目次は章見出し単位を基本とする
- 参考情報は公式情報優先で転記する
- 発行日は 2026年5月13日を仮設定として保持する
- Hermes Agent の最新情報は 2026年5月14日時点の公式 docs / release を基準にする

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C / D / E / F / G / H の参照表記が本文と一致しているか
- 図表番号と図表キャプションが本文順に並ぶか
- 本文へ入れる設定例が付録と矛盾していないか
- 前付と後付を入れたあとでページバランスが崩れないか

詳細な作業チェックは [final-assembly-checklist.md](/Users/goura32/techbook/books/hermes-agent-practical/final-assembly-checklist.md) を使う。
