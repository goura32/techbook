# Reference Sources

## 目的

- `参考情報` 欄へ最終転記する候補を、本書で使った主張と対応づけて管理する
- KDP 用の後付へ転記するときに、リンク集ではなく本文に効いている情報だけを残す

## 優先候補

### Hermes Agent

1. Hermes Agent GitHub Repository  
   種別: 公式リポジトリ  
   主な対応章: 全体  
   役割: 導入とリリースの土台  
   URL: `https://github.com/NousResearch/hermes-agent`

2. Hermes Agent Documentation  
   種別: 公式ドキュメント  
   主な対応章: 全体  
   役割: 機能説明と設定の根拠  
   URL: `https://hermes-agent.nousresearch.com/docs/`

3. Hermes Agent v0.13.0 Release Notes  
   種別: 公式リリースノート  
   日付: 2026年5月7日  
   主な対応章: 1章, 9章, 10章, 11章  
   役割: 2026年5月時点の前提と更新の速さを示す  
   URL: `https://github.com/NousResearch/hermes-agent/releases/tag/v2026.5.7`

### 主要機能

4. Profiles: Running Multiple Agents  
   種別: 公式ドキュメント  
   主な対応章: 7章  
   役割: profile の正確な役割と制約  
   URL: `https://hermes-agent.nousresearch.com/docs/user-guide/profiles/`

5. MCP (Model Context Protocol)  
   種別: 公式ドキュメント  
   主な対応章: 6章  
   役割: MCP 連携の前提  
   URL: `https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/`

6. Checkpoints and /rollback  
   種別: 公式ドキュメント  
   主な対応章: 9章  
   役割: shadow git store による rollback の根拠  
   URL: `https://hermes-agent.nousresearch.com/docs/user-guide/checkpoints-and-rollback`

## 追加候補

7. Scheduled Tasks (Cron)  
   種別: 公式ドキュメント  
   主な対応章: 10章  
   役割: cron と gateway の運用前提  
   URL: `https://hermes-agent.nousresearch.com/docs/user-guide/features/cron/`

8. Memory Providers  
   種別: 公式ドキュメント  
   主な対応章: 8章  
   役割: built-in memory と external providers の関係  
   URL: `https://hermes-agent.nousresearch.com/docs/user-guide/features/memory-providers/`

## 転記時のルール

- 後付には 8 件前後を上限目安にする
- 本文で直接使っていないものは載せない
- 日付依存の強い項目だけ公開日を併記する
- URL は最終組版直前に確認して転記する
