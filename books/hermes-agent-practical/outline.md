# Outline

## 書籍の目的

- Hermes Agent を、単なる AI コーディング支援ツールではなく、`設定` `権限` `文脈` `外部接続` `長時間運用` を持つ運用対象として理解できるようにする
- `config.yaml` `~/.hermes/.env` CLI 引数の優先順位と役割分担を明確にし、設定事故や切り分け不能な運用を避けられるようにする
- Skills、MCP、Profiles、Memory、Checkpoints、Gateway、Cron を、機能一覧ではなく「どの課題をどの層で解くか」という判断軸で整理する
- 2026年5月14日時点の公式ドキュメントを前提に、実務でまず見るべき設定項目、環境変数、CLI コマンド群を引けるようにする

## 仮タイトル

- 技術の輪郭 Hermes Agent
- サブタイトル案: 設定、Skills、MCP、Memory、Profiles、長時間運用までを実務でつなぐ

## 通しサンプル

- 題材名: `ForgeFlow`
- 概要:
  - 小規模な開発チーム向けに、Hermes Agent を `coder` `triage` `ops` の 3 profile へ分けて運用する想定例
  - CLI 利用から始め、必要に応じて Skills、MCP、Memory、Cron、Gateway を少しずつ追加する
  - 「全部を有効化する」のではなく、「どの仕事にどの能力を追加するか」を追う
- ねらい:
  - profile 分離、設定の優先順位、権限の境界、文脈の持たせ方を同じ題材で見えるようにする
  - 便利さを増やすほど観測点と停止条件も必要になることを、章ごとに確認できるようにする

## 章構成案

1. Hermes Agent をどう捉えるか
2. インストール、ディレクトリ構造、最初の設定
3. モデル、プロバイダ、権限設計
4. CLI 中心の日常ワークフロー
5. Skills と再利用可能な作業手順
6. MCP 連携の設計と運用
7. Profiles、SOUL.md、Context Files
8. Memory と文脈の持続
9. Checkpoints、変更管理、ロールバック
10. Gateway、Cron、常駐運用
11. セキュリティ、承認、事故対応
12. OpenCode / OpenClaw と比較した採用判断

## 詳細構成

### 1. Hermes Agent をどう捉えるか

- 1-1. AI チャットではなく、設定を持つ運用基盤として見る
- 1-2. Hermes Agent を構成する層
  - CLI / TUI
  - `config.yaml` `.env`
  - Skills / Profiles / Context Files
  - MCP / Memory / Checkpoints / Gateway / Cron
- 1-3. 単発利用と継続運用で何が変わるか
- 1-4. `ForgeFlow` で見る責務分離
- 1-5. 本書で扱うこと、扱わないこと

### 2. インストール、ディレクトリ構造、最初の設定

- 2-1. 2026年5月14日時点の公式導入経路
- 2-2. `~/.hermes/` 配下の構成
  - `config.yaml`
  - `.env`
  - `auth.json`
  - `SOUL.md`
  - `memories/`
  - `skills/`
  - `cron/`
  - `sessions/`
  - `logs/`
- 2-3. `hermes config` で何ができるか
- 2-4. `config.yaml` `.env` CLI 引数の優先順位
- 2-5. 最小構成の provider / model / terminal 設定
- 2-6. 導入直後によくある詰まり方

### 3. モデル、プロバイダ、権限設計

- 3-1. model と provider を分けて考える
- 3-2. provider ごとの既定値、認証、routing、timeout
- 3-3. `terminal.backend` と `terminal.cwd` が実務へ与える影響
- 3-4. ツール権限は能力ではなく責任の境界
- 3-5. approval を入れる場所、入れない場所
- 3-6. provider 差し替えより先に観測点を整える

### 4. CLI 中心の日常ワークフロー

- 4-1. `hermes chat` を土台にする理由
- 4-2. `--model` `--provider` `--skills` `--toolsets` `--worktree` `--checkpoints`
- 4-3. 毎回の依頼で揺れない入口をどう作るか
- 4-4. `--ignore-user-config` `--ignore-rules` を使った切り分け
- 4-5. 会話履歴へ依存しすぎない運用
- 4-6. CLI で安定しない手順を常駐化しない

### 5. Skills と再利用可能な作業手順

- 5-1. Skill は長いプロンプトではなく運用手順書
- 5-2. Skill に向く仕事、向かない仕事
- 5-3. profile、Skill、toolset の責務分担
- 5-4. Skill の粒度、命名、バージョン管理
- 5-5. 失敗したときに Skill をどう疑うか

### 6. MCP 連携の設計と運用

- 6-1. MCP を足す前に見るべき運用要件
- 6-2. `stdio` と `HTTP` の違い
- 6-3. `mcp_servers` の主要フィールド
  - `command`
  - `args`
  - `env`
  - `url`
  - auth
- 6-4. `hermes mcp add` `list` `test` `configure` `login`
- 6-5. 認証、接続障害、再試行、観測点
- 6-6. MCP を増やしすぎたときの運用負債

### 7. Profiles、SOUL.md、Context Files

- 7-1. profile は人格ではなく責務分離
- 7-2. `SOUL.md` に固定する原則
- 7-3. Context Files へ置くべき可変前提
- 7-4. profile ごとに何を分け、何を共有するか
- 7-5. profile を増やしすぎる失敗
- 7-6. 配布や再利用の単位として profile を見る

### 8. Memory と文脈の持続

- 8-1. built-in memory の役割
- 8-2. external memory providers が必要になる場面
- 8-3. 何を覚えさせるか、何を文書へ残すか
- 8-4. 誤記憶、古い記憶、削除方針
- 8-5. memory で設計不足を隠さない

### 9. Checkpoints、変更管理、ロールバック

- 9-1. Checkpoints をどのタスクで使うべきか
- 9-2. Git と Checkpoints の役割分担
- 9-3. `rollback` 前後で何を確認するか
- 9-4. 変更単位の切り方
- 9-5. rollback できても雑に進めない

### 10. Gateway、Cron、常駐運用

- 10-1. CLI から常駐運用へ進む条件
- 10-2. Gateway の役割
  - messaging
  - sessions
  - policy
- 10-3. Cron の役割
  - agent 実行
  - script-only 監視
  - notification
- 10-4. `hermes cron` 系コマンドとジョブ設計
- 10-5. 長時間運用で必要なログ、停止条件、ドレイン手順
- 10-6. 常駐運用へ広げる前のチェックリスト

### 11. セキュリティ、承認、事故対応

- 11-1. secrets をどこへ置くか
- 11-2. redaction、credential、ログの扱い
- 11-3. approval policy の設計
- 11-4. 外部投稿、ファイル変更、長時間ジョブの事故パターン
- 11-5. 停止、切り戻し、再開の流れ
- 11-6. 導入時と更新時の監査ポイント

### 12. OpenCode / OpenClaw と比較した採用判断

- 12-1. 比較の軸を「人気」ではなく「運用構造」に置く
- 12-2. CLI 中心利用での違い
- 12-3. Profiles、Memory、Gateway、Cron の有無で見る違い
- 12-4. Hermes Agent が向く現場、向かない現場
- 12-5. 2026年5月14日時点での採用判断の残し方

## 各章の要点

- 1章: Hermes Agent の全体像と、本書の判断軸を定義する
- 2章: 導入と設定ファイルの土台を固める
- 3章: model、provider、tool 権限、approval の線引きを扱う
- 4章: 日常の CLI 利用を安定させる
- 5章: Skills を手順資産として育てる
- 6章: MCP を安全に足す設計を扱う
- 7章: Profiles、`SOUL.md`、Context Files の責務分担を扱う
- 8章: memory を過信しない継続運用を扱う
- 9章: Checkpoints と rollback を変更管理として扱う
- 10章: Gateway と Cron を使った常駐運用を扱う
- 11章: セキュリティ、承認、事故対応を扱う
- 12章: 他ツール比較を採用判断へ戻して締める

## 付録案

### 付録 A. ForgeFlow の全体構成

- A-1. `coder` `triage` `ops` の責務分離
- A-2. profile ごとの作業範囲と toolset
- A-3. CLI から常駐運用へ広げる順番

### 付録 B. 主要機能と責務の対応

- B-1. Skills、Profiles、Context Files、Memory、MCP、Checkpoints、Cron の役割
- B-2. どの課題をどの層で解くか
- B-3. 似て見える仕組みの使い分け

### 付録 C. 設定断片と安全運用断片

- C-1. 最小構成の `config.yaml`
- C-2. provider と `.env` の分離
- C-3. MCP サーバー追加例
- C-4. profile ごとの `terminal.cwd`
- C-5. cron ジョブ例
- C-6. rollback 前提の変更例

### 付録 D. 設定キー完全表 1

- D-1. `model` / `providers` / `fallback_providers`
- D-2. `agent` / `terminal` / `browser`
- D-3. `checkpoints` / `compression` / `openrouter`
- D-4. `bedrock`
- D-5. `auxiliary`

### 付録 E. 設定キー完全表 2

- E-1. `display` / `dashboard` / `privacy`
- E-2. `tts` / `stt` / `voice`
- E-3. `human_delay` / `context` / `memory`
- E-4. `delegation` / `prefill_messages_file` / `goals`
- E-5. `skills` / `curator` / `timezone`

### 付録 F. プラットフォーム・安全性・運用キー完全表

- F-1. messaging platforms
- F-2. `approvals` / `hooks` / `quick_commands`
- F-3. `security` / `cron` / `kanban`
- F-4. `code_execution` / `logging` / `model_catalog`
- F-5. `network` / `sessions` / `updates` / `lsp`

### 付録 G. 互換キー・動的キー完全表

- G-1. `model.default` などの互換キー
- G-2. named provider と custom provider 定義
- G-3. `provider_routing`
- G-4. `mcp_servers.<name>.*`
- G-5. backend 個別キーと `platform_toolsets`

### 付録 H. 認証系環境変数とプロセス上書き一覧

- H-1. LLM / provider 認証と接続先
- H-2. messaging 認証と allowlist
- H-3. 起動時にだけ効く process-level override
- H-4. classic CLI から TUI への handoff env
- H-5. local terminal と file mutation verifier の補助 env

## 他書との役割分担メモ

- `Git & GitHub` の詳細な変更管理手法は再説明せず、agent が Git をどう扱うかに絞る
- `TypeScript` と `Python` は Hermes Agent が扱う対象例として使うが、言語解説へ逸れない
- OpenCode と OpenClaw は比較対象として扱うが、本書は Hermes Agent の運用設計を主軸にする
- UI や短命な画面導線より、設定、権限、文脈、観測、切り戻しのような古びにくい論点を優先する
