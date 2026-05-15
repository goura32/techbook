# 付録 E. 設定キー完全表 2

## この付録の役割

この付録は、2026年5月14日時点の公開 docs と、公式ソースコード `NousResearch/hermes-agent` の最新 clone を突き合わせて作成した設定キー完全表です。

- ソース基準 commit: `0f0e20ef8170`
- 主な根拠: `hermes_cli/config.py` の `DEFAULT_CONFIG`、`cli.py` と `gateway/run.py` の config→env bridge、`website/docs/reference/*.md`、`website/docs/user-guide/configuration.md`
- 範囲: 表示、音声、記憶、委譲、Skills まわりの設定
- ここでの「関連 env」は、そのキーを直接上書きする実装・互換 env を優先して記載します。provider 認証用の API key 群や messaging の bot token 群のように、周辺的だが直接 1 対 1 で対応しないものは付録 H へ分けています。

## 表示と TUI / gateway 出力

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `display.compact` | `true` / `false` | `false` | — | — | — |
| `display.personality` | 文字列 | `kawaii` | — | — | 見た目寄りの legacy 設定。本文では強く依存しない。 |
| `display.resume_display` | `full` / `minimal` | `full` | — | — | — |
| `display.busy_input_mode` | `interrupt` / `queue` / `steer` | `interrupt` | — | — | — |
| `display.tui_auto_resume_recent` | `true` / `false` | `false` | — | — | — |
| `display.bell_on_complete` | `true` / `false` | `false` | — | — | — |
| `display.show_reasoning` | `true` / `false` | `false` | — | — | — |
| `display.streaming` | `true` / `false` | `false` | — | — | — |
| `display.timestamps` | `true` / `false` | `false` | — | — | — |
| `display.final_response_markdown` | `render` / `strip` / `raw` | `strip` | — | — | — |
| `display.persistent_output` | `true` / `false` | `true` | — | — | — |
| `display.persistent_output_max_lines` | 整数 | `200` | — | — | — |
| `display.inline_diffs` | `true` / `false` | `true` | — | — | — |
| `display.file_mutation_verifier` | `true` / `false` | `true` | `HERMES_FILE_MUTATION_VERIFIER` | — | — |
| `display.show_cost` | `true` / `false` | `false` | — | — | — |
| `display.skin` | 文字列 | `default` | — | — | — |
| `display.language` | `en` / `zh` / `ja` / `de` / `es` / `fr` / `tr` / `uk` | `en` | `HERMES_LANGUAGE` | — | — |
| `display.tui_status_indicator` | `kaomoji` / `emoji` / `unicode` / `ascii` | `kaomoji` | — | — | — |
| `display.user_message_preview.first_lines` | 整数 | `2` | — | — | — |
| `display.user_message_preview.last_lines` | 整数 | `2` | — | — | — |
| `display.interim_assistant_messages` | `true` / `false` | `true` | — | — | — |
| `display.tool_progress_command` | `true` / `false` | `false` | — | — | — |
| `display.tool_progress_overrides` | マップ / オブジェクト | `{}` | — | — | — |
| `display.tool_preview_length` | 整数 | `0` | — | — | — |
| `display.ephemeral_system_ttl` | 整数 | `0` | — | — | — |
| `display.platforms` | マップ / オブジェクト | `{}` | — | — | — |
| `display.runtime_footer.enabled` | `true` / `false` | `false` | — | — | — |
| `display.runtime_footer.fields` | `model` / `context_pct` / `cwd` / `duration` / `tokens` / `cost` の配列 | `["model", "context_pct", "cwd"]` | — | — | gateway 最終返信に付くフッター項目。 |
| `display.copy_shortcut` | `auto` / `ctrl_c` / `ctrl_shift_c` / `disabled` | `auto` | — | — | — |

## dashboard

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `dashboard.theme` | 文字列 | `default` | — | — | — |
| `dashboard.show_token_analytics` | `true` / `false` | `false` | — | — | — |

## privacy

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `privacy.redact_pii` | `true` / `false` | `false` | — | — | — |

## text-to-speech

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `tts.provider` | `edge` / `elevenlabs` / `openai` / `xai` / `minimax` / `mistral` / `gemini` / `neutts` / `kittentts` / `piper` | `edge` | — | — | — |
| `tts.edge.voice` | 文字列 | `en-US-AriaNeural` | — | — | — |
| `tts.elevenlabs.voice_id` | 文字列 | `pNInz6obpgDQGcFmaJgB` | — | — | — |
| `tts.elevenlabs.model_id` | 文字列 | `eleven_multilingual_v2` | — | — | — |
| `tts.openai.model` | 文字列 | `gpt-4o-mini-tts` | — | — | — |
| `tts.openai.voice` | 文字列 | `alloy` | — | — | — |
| `tts.xai.voice_id` | 文字列 | `eve` | — | — | — |
| `tts.xai.language` | 文字列 | `en` | — | — | — |
| `tts.xai.sample_rate` | 整数 | `24000` | — | — | — |
| `tts.xai.bit_rate` | 整数 | `128000` | — | — | — |
| `tts.mistral.model` | 文字列 | `voxtral-mini-tts-2603` | — | — | — |
| `tts.mistral.voice_id` | 文字列 | `c69964a6-ab8b-4f8a-9465-ec0925096ec8` | — | — | — |
| `tts.neutts.ref_audio` | 文字列 / 空文字可 | `""` | — | — | — |
| `tts.neutts.ref_text` | 文字列 / 空文字可 | `""` | — | — | — |
| `tts.neutts.model` | 文字列 | `neuphonic/neutts-air-q4-gguf` | — | — | — |
| `tts.neutts.device` | 文字列 | `cpu` | — | — | — |
| `tts.piper.voice` | 文字列 | `en_US-lessac-medium` | — | — | — |

## speech-to-text

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `stt.enabled` | `true` / `false` | `true` | — | — | — |
| `stt.provider` | `local` / `groq` / `openai` / `mistral` | `local` | — | — | — |
| `stt.local.model` | 文字列 | `base` | — | — | — |
| `stt.local.language` | 文字列 / 空文字可 | `""` | — | — | — |
| `stt.openai.model` | 文字列 | `whisper-1` | — | — | — |
| `stt.mistral.model` | 文字列 | `voxtral-mini-latest` | — | — | — |

## voice mode

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `voice.record_key` | 文字列 | `ctrl+b` | — | — | — |
| `voice.max_recording_seconds` | 整数 | `120` | — | — | — |
| `voice.auto_tts` | `true` / `false` | `false` | — | — | — |
| `voice.beep_enabled` | `true` / `false` | `true` | — | — | — |
| `voice.silence_threshold` | 整数 | `200` | — | — | — |
| `voice.silence_duration` | 数値 | `3.0` | — | — | — |

## humanized delay

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `human_delay.mode` | `off` / `natural` / `custom` | `off` | — | — | — |
| `human_delay.min_ms` | 整数 | `800` | — | — | — |
| `human_delay.max_ms` | 整数 | `2500` | — | — | — |

## context engine

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `context.engine` | `compressor` または plugin 名 | `compressor` | — | — | — |

## memory

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `memory.memory_enabled` | `true` / `false` | `true` | — | — | — |
| `memory.user_profile_enabled` | `true` / `false` | `true` | — | — | — |
| `memory.memory_char_limit` | 整数 | `2200` | — | — | — |
| `memory.user_char_limit` | 整数 | `1375` | — | — | — |
| `memory.provider` | 文字列 / 空文字可 | `""` | — | — | 外部 memory provider は 1 つだけ有効化する前提。 |

## subagent delegation

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `delegation.model` | 文字列 / 空文字可 | `""` | — | 主に `config.yaml` / 画面設定。CLI 直接指定なし | — |
| `delegation.provider` | 文字列 / 空文字可 | `""` | — | 主に `config.yaml` / 画面設定。CLI 直接指定なし | — |
| `delegation.base_url` | 文字列 / 空文字可 | `""` | — | — | — |
| `delegation.api_key` | 文字列 / 空文字可 | `""` | — | — | — |
| `delegation.inherit_mcp_toolsets` | `true` / `false` | `true` | — | — | — |
| `delegation.max_iterations` | 整数 | `50` | — | — | — |
| `delegation.child_timeout_seconds` | 整数 | `600` | — | — | — |
| `delegation.reasoning_effort` | 文字列 / 空文字可 | `""` | — | — | — |
| `delegation.max_concurrent_children` | 整数 | `3` | — | — | — |
| `delegation.max_spawn_depth` | 整数 | `1` | — | — | — |
| `delegation.orchestrator_enabled` | `true` / `false` | `true` | — | — | — |
| `delegation.subagent_auto_approve` | `true` / `false` | `false` | — | — | subagent の dangerous command 承認を非対話で処理する。 |

## prefill messages

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `prefill_messages_file` | 文字列 / 空文字可 | `""` | `HERMES_PREFILL_MESSAGES_FILE` | — | — |

## goals

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `goals.max_turns` | 整数 | `20` | — | — | — |

## skills

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `skills.external_dirs` | 文字列配列 | `[]` | — | `hermes skills` / `hermes config set ...` | — |
| `skills.template_vars` | `true` / `false` | `true` | — | `hermes skills` / `hermes config set ...` | — |
| `skills.inline_shell` | `true` / `false` | `false` | — | `hermes skills` / `hermes config set ...` | — |
| `skills.inline_shell_timeout` | 整数 | `10` | — | `hermes skills` / `hermes config set ...` | — |
| `skills.guard_agent_created` | `true` / `false` | `false` | — | `hermes skills` / `hermes config set ...` | — |

## curator

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `curator.enabled` | `true` / `false` | `true` | — | — | — |
| `curator.interval_hours` | 整数 | `168` | — | — | — |
| `curator.min_idle_hours` | 整数 | `2` | — | — | — |
| `curator.stale_after_days` | 整数 | `30` | — | — | — |
| `curator.archive_after_days` | 整数 | `90` | — | — | — |
| `curator.backup.enabled` | `true` / `false` | `true` | — | — | — |
| `curator.backup.keep` | 整数 | `5` | — | — | — |

## Honcho overrides

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `honcho` | マップ / オブジェクト | `{}` | — | — | — |

## timezone

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `timezone` | 文字列 / 空文字可 | `""` | `HERMES_TIMEZONE` | — | — |

## 読み方

- `関連 env` が `—` のものは、通常 `config.yaml` でだけ扱うキーです。
- `関連 CLI / コマンド` が `hermes config set ...` 中心になっているものは、専用フラグより設定ファイル運用が本筋です。
- 動的マップや旧互換キーは付録 G、認証系 env とプロセス全体へ効く env / CLI スイッチは付録 H を参照してください。
