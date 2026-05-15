# Figures

## 優先度の高い図表

### 図1-1 Hermes Agent の構造

- 章: 1章
- ねらい: CLI、設定、Profiles、Skills、MCP、Memory、Checkpoints、Gateway、Cron の層を一目で示す

### 図2-1 `~/.hermes/` 配下の構成

- 章: 2章
- ねらい: `config.yaml` `.env` `auth.json` `memories/` `cron/` `sessions/` `logs/` の位置づけを示す

### 表2-1 設定の優先順位と置き場所

- 章: 2章
- ねらい: CLI 引数、`config.yaml`、`.env`、built-in defaults の違いを示す

### 図3-1 model、provider、tool permissions の関係

- 章: 3章
- ねらい: モデル選定と権限設計が別の判断であることを示す

### 表3-1 よく触る設定と秘密情報の分け方

- 章: 3章
- ねらい: `config.yaml` と `.env` の責務分担を示す

### 表4-1 `hermes chat` の主要引数

- 章: 4章
- ねらい: 日常利用でまず使う引数と、一時切り分け用の引数を整理する

### 表5-1 Skill に切り出すべき手順の特徴

- 章: 5章
- ねらい: スキル化する価値のある仕事を整理する

### 図6-1 MCP で能力を足す流れ

- 章: 6章
- ねらい: Hermes 本体、MCP client、MCP server、外部 API の境界を示す

### 表6-1 `mcp_servers` の主要フィールド

- 章: 6章
- ねらい: `command` `args` `env` `url` auth の役割を引けるようにする

### 表7-1 Profiles、SOUL.md、Context Files の役割

- 章: 7章
- ねらい: 混同しやすい構成要素を責務で分ける

### 図8-1 built-in memory と external providers の関係

- 章: 8章
- ねらい: memory の位置づけと、文書・memory・external provider の役割分担を示す

### 図9-1 Checkpoints と rollback の流れ

- 章: 9章
- ねらい: 変更前、変更後、checkpoint、rollback の関係を示す

### 図10-1 Gateway と Cron の長時間運用

- 章: 10章
- ねらい: gateway daemon、sessions、cron jobs、通知先の関係を示す

### 表10-1 常駐運用へ進む前のチェックリスト

- 章: 10章
- ねらい: CLI で安定していない構成を常駐化しないための確認項目を残す

### 表11-1 セキュリティと承認のチェックリスト

- 章: 11章
- ねらい: secrets、approval、redaction、logging、rollback の最低限の確認項目を残す

### 表12-1 Hermes / OpenCode / OpenClaw の比較

- 章: 12章
- ねらい: 採用判断の軸を整理する
