# 付録 D. 設定キー完全表 1

## この付録の役割

この付録は、2026年5月14日時点の公開 docs と、公式ソースコード `NousResearch/hermes-agent` の最新 clone を突き合わせて作成した設定キー完全表です。

- ソース基準 commit: `0f0e20ef8170`
- 主な根拠: `hermes_cli/config.py` の `DEFAULT_CONFIG`、`cli.py` と `gateway/run.py` の config→env bridge、`website/docs/reference/*.md`、`website/docs/user-guide/configuration.md`
- 範囲: model から auxiliary までの中核設定
- ここでの「関連 env」は、そのキーを直接上書きする実装・互換 env を優先して記載します。provider 認証用の API key 群や messaging の bot token 群のように、周辺的だが直接 1 対 1 で対応しないものは付録 H へ分けています。

## モデルとプロバイダの基本

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `model` | 文字列 / 空文字可 | `""` | `HERMES_MODEL` | `hermes chat --model` / `hermes model` | 主設定は root の `model:` 文字列。`model.default` などの互換キーは付録 G を参照。 |

## 命名プロバイダ定義

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `providers` | マップ / オブジェクト | `{}` | — | — | named custom providers を格納する動的マップ。詳細は付録 G。 |

## フォールバックチェーン

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `fallback_providers` | 配列 | `[]` | — | — | 新しい fallback chain。旧 `fallback_model` は付録 G。 |

## 認証プール戦略

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `credential_pool_strategies` | マップ / オブジェクト | `{}` | — | — | — |

## 有効ツールセット

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `toolsets` | toolset 名の文字列配列 | `["hermes-cli"]` | — | `hermes chat --toolsets` | `hermes tools` で更新されることが多い。 |

## エージェントの挙動

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `agent.max_turns` | 整数 | `90` | `HERMES_MAX_ITERATIONS` | `hermes chat --max-turns` | — |
| `agent.gateway_timeout` | 整数 | `1800` | — | — | — |
| `agent.restart_drain_timeout` | 整数 | `180` | — | — | — |
| `agent.api_max_retries` | 整数 | `3` | — | — | — |
| `agent.service_tier` | 文字列 / 空文字可 | `""` | — | — | — |
| `agent.tool_use_enforcement` | "auto" / `true` / `false` / 文字列配列 | `auto` | — | — | 配列を使うとモデル名部分一致で適用する。 |
| `agent.gateway_timeout_warning` | 整数 | `900` | — | — | — |
| `agent.clarify_timeout` | 整数 | `600` | — | — | — |
| `agent.gateway_notify_interval` | 整数 | `180` | — | — | — |
| `agent.gateway_auto_continue_freshness` | 整数 | `3600` | — | — | — |
| `agent.image_input_mode` | `auto` / `native` / `text` | `auto` | — | — | 画像入力をネイティブに渡すか、先に説明文へ変換するかを決める。 |
| `agent.disabled_toolsets` | toolset 名の文字列配列 | `[]` | — | — | — |

## terminal backend

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `terminal.backend` | `local` / `docker` / `ssh` / `singularity` / `modal` / `daytona` / `vercel_sandbox` | `local` | `TERMINAL_ENV` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.modal_mode` | `auto` / `managed` / `direct` | `auto` | `TERMINAL_MODAL_MODE` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.cwd` | 文字列 | `.` | — | `hermes setup terminal` / `hermes config set ...` | CLI は起動ディレクトリ優先。gateway / cron ではこの値が効く。 |
| `terminal.timeout` | 整数 | `180` | `TERMINAL_TIMEOUT` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.env_passthrough` | 配列 | `[]` | — | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.shell_init_files` | 配列 | `[]` | — | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.auto_source_bashrc` | `true` / `false` | `true` | — | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_image` | 文字列 | `nikolaik/python-nodejs:python3.11-nodejs20` | `TERMINAL_DOCKER_IMAGE` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_forward_env` | 配列 | `[]` | `TERMINAL_DOCKER_FORWARD_ENV` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_env` | マップ / オブジェクト | `{}` | — | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.singularity_image` | 文字列 | `docker://nikolaik/python-nodejs:python3.11-nodejs20` | `TERMINAL_SINGULARITY_IMAGE` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.modal_image` | 文字列 | `nikolaik/python-nodejs:python3.11-nodejs20` | `TERMINAL_MODAL_IMAGE` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.daytona_image` | 文字列 | `nikolaik/python-nodejs:python3.11-nodejs20` | `TERMINAL_DAYTONA_IMAGE` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.vercel_runtime` | 文字列 | `node24` | `TERMINAL_VERCEL_RUNTIME` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.container_cpu` | 整数 | `1` | `TERMINAL_CONTAINER_CPU` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.container_memory` | 整数 | `5120` | `TERMINAL_CONTAINER_MEMORY` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.container_disk` | 整数 | `51200` | `TERMINAL_CONTAINER_DISK` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.container_persistent` | `true` / `false` | `true` | `TERMINAL_CONTAINER_PERSISTENT` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_volumes` | 配列 | `[]` | — | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_mount_cwd_to_workspace` | `true` / `false` | `false` | `TERMINAL_DOCKER_MOUNT_CWD_TO_WORKSPACE` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_extra_args` | 配列 | `[]` | — | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.docker_run_as_host_user` | `true` / `false` | `false` | `TERMINAL_DOCKER_RUN_AS_HOST_USER` | `hermes setup terminal` / `hermes config set ...` | — |
| `terminal.persistent_shell` | `true` / `false` | `true` | `TERMINAL_PERSISTENT_SHELL` | `hermes setup terminal` / `hermes config set ...` | — |

## web backend

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `web.backend` | 文字列 / 空文字可 | `""` | — | — | — |
| `web.search_backend` | 文字列 / 空文字可 | `""` | — | — | — |
| `web.extract_backend` | 文字列 / 空文字可 | `""` | — | — | — |

## browser と CDP

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `browser.inactivity_timeout` | 整数 | `120` | `BROWSER_INACTIVITY_TIMEOUT` | `hermes tools` / `hermes config set ...` | — |
| `browser.command_timeout` | 整数 | `30` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.record_sessions` | `true` / `false` | `false` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.allow_private_urls` | `true` / `false` | `false` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.engine` | `auto` / `lightpanda` / `chrome` | `auto` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.auto_local_for_private_urls` | `true` / `false` | `true` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.cdp_url` | 文字列 / 空文字可 | `""` | `BROWSER_CDP_URL` | `hermes tools` / `hermes config set ...` | `/browser connect` で使う既存 Chrome への接続先。 |
| `browser.dialog_policy` | `must_respond` / `auto_dismiss` / `auto_accept` | `must_respond` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.dialog_timeout_s` | 整数 | `300` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.camofox.managed_persistence` | `true` / `false` | `false` | — | `hermes tools` / `hermes config set ...` | — |
| `browser.camofox.user_id` | 文字列 / 空文字可 | `""` | `CAMOFOX_USER_ID` | `hermes tools` / `hermes config set ...` | — |
| `browser.camofox.session_key` | 文字列 / 空文字可 | `""` | `CAMOFOX_SESSION_KEY` | `hermes tools` / `hermes config set ...` | — |
| `browser.camofox.adopt_existing_tab` | `true` / `false` | `false` | `CAMOFOX_ADOPT_EXISTING_TAB` | `hermes tools` / `hermes config set ...` | — |

## checkpoints

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `checkpoints.enabled` | `true` / `false` | `false` | — | `hermes chat --checkpoints` | — |
| `checkpoints.max_snapshots` | 整数 | `20` | — | — | — |
| `checkpoints.max_total_size_mb` | 整数 | `500` | — | — | — |
| `checkpoints.max_file_size_mb` | 整数 | `10` | — | — | — |
| `checkpoints.auto_prune` | `true` / `false` | `true` | — | — | — |
| `checkpoints.retention_days` | 整数 | `7` | — | — | — |
| `checkpoints.delete_orphans` | `true` / `false` | `true` | — | — | — |
| `checkpoints.min_interval_hours` | 整数 | `24` | — | — | — |

## read_file 制限

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `file_read_max_chars` | 整数 | `100000` | — | — | — |

## tool output 制限

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `tool_output.max_bytes` | 整数 | `50000` | — | — | — |
| `tool_output.max_lines` | 整数 | `2000` | — | — | — |
| `tool_output.max_line_length` | 整数 | `2000` | — | — | — |

## tool loop guardrails

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `tool_loop_guardrails.warnings_enabled` | `true` / `false` | `true` | — | — | — |
| `tool_loop_guardrails.hard_stop_enabled` | `true` / `false` | `false` | — | — | — |
| `tool_loop_guardrails.warn_after.exact_failure` | 整数 | `2` | — | — | — |
| `tool_loop_guardrails.warn_after.same_tool_failure` | 整数 | `3` | — | — | — |
| `tool_loop_guardrails.warn_after.idempotent_no_progress` | 整数 | `2` | — | — | — |
| `tool_loop_guardrails.hard_stop_after.exact_failure` | 整数 | `5` | — | — | — |
| `tool_loop_guardrails.hard_stop_after.same_tool_failure` | 整数 | `8` | — | — | — |
| `tool_loop_guardrails.hard_stop_after.idempotent_no_progress` | 整数 | `5` | — | — | — |

## context compression

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `compression.enabled` | `true` / `false` | `true` | — | — | — |
| `compression.threshold` | 数値 | `0.5` | — | — | — |
| `compression.target_ratio` | 数値 | `0.2` | — | — | — |
| `compression.protect_last_n` | 整数 | `20` | — | — | — |
| `compression.hygiene_hard_message_limit` | 整数 | `400` | — | — | — |
| `compression.protect_first_n` | 整数 | `3` | — | — | — |

## prompt caching

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `prompt_caching.cache_ttl` | 文字列 | `5m` | — | — | — |

## OpenRouter 固有設定

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `openrouter.response_cache` | `true` / `false` | `true` | `HERMES_OPENROUTER_CACHE` | — | — |
| `openrouter.response_cache_ttl` | 整数 | `300` | `HERMES_OPENROUTER_CACHE_TTL` | — | — |
| `openrouter.min_coding_score` | 数値 | `0.65` | — | — | — |

## AWS Bedrock

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `bedrock.region` | 文字列 / 空文字可 | `""` | — | — | — |
| `bedrock.discovery.enabled` | `true` / `false` | `true` | — | — | — |
| `bedrock.discovery.provider_filter` | 配列 | `[]` | — | — | — |
| `bedrock.discovery.refresh_interval` | 整数 | `3600` | — | — | — |
| `bedrock.guardrail.guardrail_identifier` | 文字列 / 空文字可 | `""` | — | — | — |
| `bedrock.guardrail.guardrail_version` | 文字列 / 空文字可 | `""` | — | — | — |
| `bedrock.guardrail.stream_processing_mode` | `sync` / `async` | `async` | — | — | — |
| `bedrock.guardrail.trace` | `disabled` / `enabled` / `enabled_full` | `disabled` | — | — | — |

## auxiliary tasks

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `auxiliary.vision.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | `AUXILIARY_VISION_PROVIDER` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.vision.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_VISION_MODEL` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.vision.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_VISION_BASE_URL` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.vision.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_VISION_API_KEY` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.vision.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `120` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.vision.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.vision.download_timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `30` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.web_extract.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | `AUXILIARY_WEB_EXTRACT_PROVIDER` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.web_extract.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_WEB_EXTRACT_MODEL` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.web_extract.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_WEB_EXTRACT_BASE_URL` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.web_extract.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_WEB_EXTRACT_API_KEY` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.web_extract.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `360` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.web_extract.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.compression.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.compression.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.compression.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.compression.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.compression.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `120` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.compression.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `30` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.session_search.max_concurrency` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `3` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.skills_hub.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.skills_hub.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.skills_hub.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.skills_hub.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.skills_hub.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `30` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.skills_hub.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.approval.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | `AUXILIARY_APPROVAL_PROVIDER` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.approval.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_APPROVAL_MODEL` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.approval.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_APPROVAL_BASE_URL` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.approval.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | `AUXILIARY_APPROVAL_API_KEY` | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.approval.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `30` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.approval.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.mcp.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.mcp.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.mcp.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.mcp.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.mcp.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `30` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.mcp.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.title_generation.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.title_generation.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.title_generation.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.title_generation.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.title_generation.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `30` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.title_generation.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.triage_specifier.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.triage_specifier.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.triage_specifier.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.triage_specifier.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.triage_specifier.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `120` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.triage_specifier.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.curator.provider` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `auto` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.curator.model` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.curator.base_url` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.curator.api_key` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `""` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.curator.timeout` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `600` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |
| `auxiliary.curator.extra_body` | `auto` / `main` / built-in provider 名 / custom provider 名 / 直接 endpoint | `{}` | — | `hermes setup model` / `hermes config set ...` | task ごとに provider / model / base_url / api_key を独立指定できる。 |

## 読み方

- `関連 env` が `—` のものは、通常 `config.yaml` でだけ扱うキーです。
- `関連 CLI / コマンド` が `hermes config set ...` 中心になっているものは、専用フラグより設定ファイル運用が本筋です。
- 動的マップや旧互換キーは付録 G、認証系 env とプロセス全体へ効く env / CLI スイッチは付録 H を参照してください。
