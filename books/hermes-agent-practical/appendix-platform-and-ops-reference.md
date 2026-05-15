# 付録 F. プラットフォーム・安全性・運用キー完全表

## この付録の役割

この付録は、2026年5月14日時点の公開 docs と、公式ソースコード `NousResearch/hermes-agent` の最新 clone を突き合わせて作成した設定キー完全表です。

- ソース基準 commit: `0f0e20ef8170`
- 主な根拠: `hermes_cli/config.py` の `DEFAULT_CONFIG`、`cli.py` と `gateway/run.py` の config→env bridge、`website/docs/reference/*.md`、`website/docs/user-guide/configuration.md`
- 範囲: messaging、承認、セキュリティ、保守運用の設定
- ここでの「関連 env」は、そのキーを直接上書きする実装・互換 env を優先して記載します。provider 認証用の API key 群や messaging の bot token 群のように、周辺的だが直接 1 対 1 で対応しないものは付録 H へ分けています。

## Slack

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `slack.require_mention` | `true` / `false` | `true` | `SLACK_BOT_TOKEN`, `SLACK_APP_TOKEN` | `hermes gateway setup slack` / `hermes config set ...` | 認証自体は env、挙動は config で決める。 |
| `slack.free_response_channels` | 文字列 / 空文字可 | `""` | `SLACK_BOT_TOKEN`, `SLACK_APP_TOKEN` | `hermes gateway setup slack` / `hermes config set ...` | — |
| `slack.allowed_channels` | 文字列 / 空文字可 | `""` | `SLACK_BOT_TOKEN`, `SLACK_APP_TOKEN` | `hermes gateway setup slack` / `hermes config set ...` | allowlist の初期投入は setup で済ませることが多い。 |
| `slack.channel_prompts` | マップ / オブジェクト | `{}` | `SLACK_BOT_TOKEN`, `SLACK_APP_TOKEN` | `hermes gateway setup slack` / `hermes config set ...` | channel 単位の一時 system prompt。 |

## Discord

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `discord.require_mention` | `true` / `false` | `true` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | 認証は bot token で行う。 |
| `discord.free_response_channels` | 文字列 / 空文字可 | `""` | `DISCORD_BOT_TOKEN`, `DISCORD_ALLOWED_CHANNELS` | `hermes gateway setup discord` / `hermes config set ...` | — |
| `discord.allowed_channels` | 文字列 / 空文字可 | `""` | `DISCORD_BOT_TOKEN`, `DISCORD_ALLOWED_CHANNELS` | `hermes gateway setup discord` / `hermes config set ...` | env と config が同じ概念を持つ。 |
| `discord.auto_thread` | `true` / `false` | `true` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | — |
| `discord.thread_require_mention` | `true` / `false` | `false` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | — |
| `discord.reactions` | `true` / `false` | `true` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | — |
| `discord.channel_prompts` | マップ / オブジェクト | `{}` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | channel / thread 単位の一時 system prompt。 |
| `discord.dm_role_auth_guild` | 文字列 / 空文字可 | `""` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | — |
| `discord.server_actions` | 文字列 / 空文字可 | `""` | `DISCORD_BOT_TOKEN` | `hermes gateway setup discord` / `hermes config set ...` | — |

## WhatsApp

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `whatsapp` | マップ / オブジェクト | `{}` | `WHATSAPP_ENABLED`, `WHATSAPP_MODE`, `WHATSAPP_ALLOWED_USERS`, `WHATSAPP_ALLOW_ALL_USERS`, `WHATSAPP_DEBUG`, `WHATSAPP_HOME_CHANNEL` | `hermes gateway setup whatsapp` / `hermes config set ...` | 実装依存の gateway 設定群。詳細な認証・接続系 env は付録 H。 |

## Telegram

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `telegram.reactions` | `true` / `false` | `false` | `TELEGRAM_BOT_TOKEN` | `hermes gateway setup telegram` / `hermes config set ...` | — |
| `telegram.channel_prompts` | マップ / オブジェクト | `{}` | `TELEGRAM_BOT_TOKEN` | `hermes gateway setup telegram` / `hermes config set ...` | chat / topic 単位の一時 system prompt。 |
| `telegram.allowed_chats` | 文字列 / 空文字可 | `""` | `TELEGRAM_BOT_TOKEN`, `TELEGRAM_GROUP_ALLOWED_CHATS` | `hermes gateway setup telegram` / `hermes config set ...` | env と config が同じ概念を持つ。 |

## Mattermost

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `mattermost.require_mention` | `true` / `false` | `true` | `MATTERMOST_URL`, `MATTERMOST_TOKEN` | `hermes gateway setup mattermost` / `hermes config set ...` | — |
| `mattermost.free_response_channels` | 文字列 / 空文字可 | `""` | `MATTERMOST_URL`, `MATTERMOST_TOKEN` | `hermes gateway setup mattermost` / `hermes config set ...` | — |
| `mattermost.allowed_channels` | 文字列 / 空文字可 | `""` | `MATTERMOST_URL`, `MATTERMOST_TOKEN` | `hermes gateway setup mattermost` / `hermes config set ...` | — |
| `mattermost.channel_prompts` | マップ / オブジェクト | `{}` | `MATTERMOST_URL`, `MATTERMOST_TOKEN` | `hermes gateway setup mattermost` / `hermes config set ...` | — |

## Matrix

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `matrix.require_mention` | `true` / `false` | `true` | `MATRIX_HOMESERVER`, `MATRIX_ACCESS_TOKEN`, `MATRIX_HOME_ROOM` | `hermes gateway setup matrix` / `hermes config set ...` | — |
| `matrix.free_response_rooms` | 文字列 / 空文字可 | `""` | `MATRIX_HOMESERVER`, `MATRIX_ACCESS_TOKEN`, `MATRIX_HOME_ROOM` | `hermes gateway setup matrix` / `hermes config set ...` | — |
| `matrix.allowed_rooms` | 文字列 / 空文字可 | `""` | `MATRIX_HOMESERVER`, `MATRIX_ACCESS_TOKEN`, `MATRIX_HOME_ROOM` | `hermes gateway setup matrix` / `hermes config set ...` | — |

## approvals

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `approvals.mode` | `manual` / `smart` / `off` | `manual` | — | CLI 直接 flag は限定的。通常は `config.yaml` | — |
| `approvals.timeout` | 整数 | `60` | — | CLI 直接 flag は限定的。通常は `config.yaml` | — |
| `approvals.cron_mode` | `deny` / `approve` | `deny` | — | CLI 直接 flag は限定的。通常は `config.yaml` | — |
| `approvals.mcp_reload_confirm` | `true` / `false` | `true` | — | CLI 直接 flag は限定的。通常は `config.yaml` | — |
| `approvals.destructive_slash_confirm` | `true` / `false` | `true` | — | CLI 直接 flag は限定的。通常は `config.yaml` | — |

## command allowlist

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `command_allowlist` | 配列 | `[]` | — | — | — |

## quick commands

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `quick_commands` | マップ / オブジェクト | `{}` | — | — | agent loop を経由しない即時実行コマンドのマップ。 |

## hooks

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `hooks` | マップ / オブジェクト | `{}` | — | — | event 名 -> shell command 群のマップ。新規登録承認が必要。 |

## hook registration approval

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `hooks_auto_accept` | `true` / `false` | `false` | — | — | — |

## custom personalities

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `personalities` | マップ / オブジェクト | `{}` | — | — | — |

## security

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `security.allow_private_urls` | `true` / `false` | `false` | — | — | — |
| `security.redact_secrets` | `true` / `false` | `true` | `HERMES_REDACT_SECRETS` | — | docs と config comments の既定値説明に差分があるため、ソース既定値を採用。 |
| `security.tirith_enabled` | `true` / `false` | `true` | — | — | — |
| `security.tirith_path` | 文字列 | `tirith` | — | — | — |
| `security.tirith_timeout` | 整数 | `5` | — | — | — |
| `security.tirith_fail_open` | `true` / `false` | `true` | — | — | — |
| `security.website_blocklist.enabled` | `true` / `false` | `false` | — | — | web / browser 系ツールの URL 制御に使う。 |
| `security.website_blocklist.domains` | 文字列配列 | `[]` | — | — | web / browser 系ツールの URL 制御に使う。 |
| `security.website_blocklist.shared_files` | 文字列配列 | `[]` | — | — | web / browser 系ツールの URL 制御に使う。 |
| `security.acked_advisories` | 配列 | `[]` | — | — | — |
| `security.allow_lazy_installs` | `true` / `false` | `true` | — | — | — |

## cron

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `cron.wrap_response` | `true` / `false` | `true` | — | — | — |
| `cron.max_parallel_jobs` | `null` または数値 / 文字列 | `null` | `HERMES_CRON_MAX_PARALLEL` | — | — |

## kanban dispatcher

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `kanban.dispatch_in_gateway` | `true` / `false` | `true` | — | — | — |
| `kanban.dispatch_interval_seconds` | 整数 | `60` | — | — | — |
| `kanban.failure_limit` | 整数 | `2` | — | — | — |

## execute_code

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `code_execution.mode` | `project` / `strict` | `project` | — | — | — |

## logging

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `logging.level` | `DEBUG` / `INFO` / `WARNING` | `INFO` | — | — | — |
| `logging.max_size_mb` | 整数 | `5` | — | — | — |
| `logging.backup_count` | 整数 | `3` | — | — | — |

## model catalog

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `model_catalog.enabled` | `true` / `false` | `true` | — | — | — |
| `model_catalog.url` | 文字列 | `https://hermes-agent.nousresearch.com/docs/api/model-catalog.json` | — | — | remote curated model list の配布先。 |
| `model_catalog.ttl_hours` | 整数 | `24` | — | — | — |
| `model_catalog.providers` | マップ / オブジェクト | `{}` | — | — | — |

## network

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `network.force_ipv4` | `true` / `false` | `false` | — | — | — |

## sessions maintenance

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `sessions.auto_prune` | `true` / `false` | `false` | — | — | — |
| `sessions.retention_days` | 整数 | `90` | — | — | — |
| `sessions.vacuum_after_prune` | `true` / `false` | `true` | — | — | — |
| `sessions.min_interval_hours` | 整数 | `24` | — | — | — |

## onboarding hints

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `onboarding.seen` | マップ / オブジェクト | `{}` | — | — | — |

## update behavior

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `updates.pre_update_backup` | `true` / `false` | `false` | — | — | — |
| `updates.backup_keep` | 整数 | `5` | — | — | — |

## LSP

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `lsp.enabled` | `true` / `false` | `true` | — | — | — |
| `lsp.wait_mode` | `document` / `full` | `document` | — | — | — |
| `lsp.wait_timeout` | 数値 | `5.0` | — | — | — |
| `lsp.install_strategy` | `auto` / `manual` / `off` | `auto` | — | — | — |
| `lsp.servers` | マップ / オブジェクト | `{}` | — | — | server id ごとの override マップ。具体形は表でなく付録 G へ。 |

## 設定スキーマ版

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `_config_version` | 整数 | `23` | — | — | 内部移行用。通常は手で変更しない。 |

## platform plugin extra config

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `platforms` | マップ / オブジェクト | `{}` | — | — | plugin platform が独自の `extra` マップを差し込む場所。 |

## 読み方

- `関連 env` が `—` のものは、通常 `config.yaml` でだけ扱うキーです。
- `関連 CLI / コマンド` が `hermes config set ...` 中心になっているものは、専用フラグより設定ファイル運用が本筋です。
- 動的マップや旧互換キーは付録 G、認証系 env とプロセス全体へ効く env / CLI スイッチは付録 H を参照してください。
