# Outline

## 書籍の目的

- Hermes Agent を単なる話題の AI エージェントとしてではなく、継続運用できる開発支援基盤として理解できるようにする
- インストール、モデル設定、ツール権限、スキル、MCP、メモリ、Cron、メッセージングまでを一貫した運用視点で整理する

## 仮タイトル

- 技術の輪郭 Hermes Agent
- サブタイトル案: Skills、MCP、Memory、Profiles、運用設計までを実務でつなぐ

## 章構成案

1. Hermes Agent とは何か
2. インストールと最初のセットアップ
3. モデル、プロバイダ、ツール権限の考え方
4. CLI を中心とした日常ワークフロー
5. Skills と再利用可能な手順設計
6. MCP 連携で広げる能力
7. Profiles、SOUL.md、Context Files の設計
8. Memory、Memory Providers、文脈の持続
9. Checkpoints と安全な変更
10. Gateway、Cron、長時間運用
11. セキュリティ、承認、運用事故への備え
12. OpenCode / OpenClaw との比較と使い分け

## 各章の要点

- 1章: Hermes Agent の立ち位置と他エージェントとの違いを整理する
- 2章: 初期導入で詰まりやすい点をまとめる
- 3章: モデル選定、権限、実行環境の考え方を扱う
- 4章: 日常利用の実践フローを扱う
- 5章: Skills による手順資産化を扱う
- 6章: MCP を通じた拡張と設計上の注意を扱う
- 7章: profile 分離、`SOUL.md`、プロジェクト文脈ファイルの設計を扱う
- 8章: 組み込み memory と外部 memory providers、誤学習リスクを扱う
- 9章: filesystem checkpoints と変更の巻き戻し戦略を扱う
- 10章: Gateway、Cron、通知先、長時間運用の設計を扱う
- 11章: defense-in-depth、承認、container isolation、credential filtering を扱う
- 12章: Hermes Agent を採用すべき場面と他ツールを選ぶ場面を整理する

## 他書との役割分担メモ

- `Git & GitHub` の詳細な変更管理手法は再説明せず、必要箇所だけ参照する
- `TypeScript` と `Python` はエージェントが扱う対象コードの例として使うが、言語解説へ逸れない
- OpenCode と OpenClaw は比較対象として出すが、本書は Hermes Agent 中心で構成する
- UI や一時的な操作手順より、古びにくい運用判断と安全設計を優先する
