# 付録 H. 認証系環境変数とプロセス上書き一覧

## この付録の役割

この付録は、`config.yaml` の設定キーに直接 1 対 1 対応しないが、実運用では必ず確認する環境変数と process-level override をまとめたものです。

- provider 認証、messaging gateway 認証、起動時だけ効く CLI / env override を分けて整理する
- `config.yaml` に保存される恒久設定は付録 D〜G、対応する設定キーは本文と付録 D〜F を参照する

## H-1. Provider 認証と接続先

| env / 引数 | 根拠 | 設定可能な値 | 役割 | 関連 CLI | 備考 |
| --- | --- | --- | --- | --- | --- |
| `OPENROUTER_API_KEY` | 公開 docs + ソース | 文字列 | OpenRouter 認証。標準の既定経路。 | `hermes model` / `hermes auth` | `model` と `fallback_providers` のどちらにも効く。 |
| `OPENAI_API_KEY` | 公開 docs + ソース | 文字列 | custom OpenAI-compatible endpoint 認証。 | `hermes model` / `hermes auth` | 通常は `OPENAI_BASE_URL` と組み合わせる。 |
| `ANTHROPIC_API_KEY` | 公開 docs + ソース | 文字列 | Anthropic API key 認証。 | `hermes model` / `hermes auth` | `ANTHROPIC_TOKEN` より通常はこちらを優先。 |
| `ANTHROPIC_TOKEN` | 公開 docs + ソース | 文字列 | Anthropic の手動 token / legacy OAuth override。 | `hermes auth` | Claude Max / Claude Code 連携の補助経路。 |
| `GOOGLE_API_KEY` / `GEMINI_API_KEY` | 公開 docs + ソース | 文字列 | Gemini / Google AI Studio 認証。 | `hermes model` / `hermes auth` | `GEMINI_API_KEY` は alias。 |
| `DEEPSEEK_API_KEY` | 公開 docs + ソース | 文字列 | DeepSeek 認証。 | `hermes model` / `hermes auth` | — |
| `XAI_API_KEY` | 公開 docs + ソース | 文字列 | xAI / Grok 認証。 | `hermes model` / `hermes auth` | chat と TTS の両方で使う。 |
| `MISTRAL_API_KEY` | 公開 docs + ソース | 文字列 | Mistral 認証。 | `hermes model` / `hermes auth` | STT / TTS まわりでも参照される。 |
| `GLM_API_KEY` / `ZAI_API_KEY` / `Z_AI_API_KEY` | 公開 docs + ソース | 文字列 | z.ai / GLM 認証。 | `hermes model` / `hermes auth` | 後ろ 2 つは alias。 |
| `KIMI_API_KEY` | 公開 docs + ソース | 文字列 | Kimi / Moonshot 認証。 | `hermes model` / `hermes auth` | 中国系 provider 群では別 base URL と組み合わせることがある。 |
| `MINIMAX_API_KEY` / `MINIMAX_CN_API_KEY` | 公開 docs + ソース | 文字列 | MiniMax 認証。 | `hermes model` / `hermes auth` | `minimax-oauth` は browser login で別経路。 |
| `NVIDIA_API_KEY` | 公開 docs + ソース | 文字列 | NVIDIA NIM 認証。 | `hermes model` / `hermes auth` | local NIM endpoint では不要なこともある。 |
| `STEPFUN_API_KEY` | 公開 docs + ソース | 文字列 | StepFun 認証。 | `hermes model` / `hermes auth` | — |
| `AZURE_FOUNDRY_API_KEY` / `AZURE_ANTHROPIC_KEY` | 公開 docs + ソース | 文字列 | Azure Foundry / Azure Anthropic 認証。 | `hermes model` / `hermes auth` | endpoint の種類に応じて使い分ける。 |
| `AI_GATEWAY_API_KEY` | 公開 docs + ソース | 文字列 | Vercel AI Gateway 認証。 | `hermes model` / `hermes auth` | — |
| `HF_TOKEN` | 公開 docs + ソース | 文字列 | Hugging Face Inference Providers 認証。 | `hermes model` / `hermes auth` | — |
| `OLLAMA_API_KEY` | 公開 docs + ソース | 文字列 | Ollama Cloud 認証。 | `hermes model` / `hermes auth` | local Ollama は通常不要。 |
| `OPENCODE_ZEN_API_KEY` / `OPENCODE_GO_API_KEY` | 公開 docs + ソース | 文字列 | OpenCode 系 provider 認証。 | `hermes model` / `hermes auth` | 用途ごとに別課金体系。 |
| `COPILOT_GITHUB_TOKEN` / `GH_TOKEN` / `GITHUB_TOKEN` | 公開 docs + ソース | 文字列 | Copilot / GitHub 認証。 | `hermes auth` | 優先順位はこの順。 |
| `OPENAI_BASE_URL` / `OPENROUTER_BASE_URL` / 各 provider `*_BASE_URL` | 公開 docs + ソース | URL | 認証そのものではないが provider 接続先を変える補助 env。 | `hermes model` | endpoint を切り替えるときに認証 env とセットで扱う。 |

## H-2. Messaging gateway 認証

| env / 引数 | 根拠 | 設定可能な値 | 役割 | 関連 CLI | 備考 |
| --- | --- | --- | --- | --- | --- |
| `SLACK_BOT_TOKEN` / `SLACK_APP_TOKEN` | 公開 docs + ソース | 文字列 | Slack gateway 認証。 | `hermes gateway setup slack` | `slack.*` キーの挙動設定とは別に必須。 |
| `DISCORD_BOT_TOKEN` | 公開 docs + ソース | 文字列 | Discord gateway 認証。 | `hermes gateway setup discord` | `DISCORD_ALLOWED_CHANNELS` などと併用する。 |
| `DISCORD_ALLOWED_CHANNELS` | 公開 docs + ソース | csv | Discord allowlist を env で注入する。 | `hermes gateway setup discord` / `hermes config set discord.allowed_channels ...` | config 側の `discord.allowed_channels` と同概念。 |
| `TELEGRAM_BOT_TOKEN` | 公開 docs + ソース | 文字列 | Telegram gateway 認証。 | `hermes gateway setup telegram` | `telegram.*` キーの挙動設定とは別に必須。 |
| `TELEGRAM_GROUP_ALLOWED_CHATS` | 公開 docs + ソース | csv | Telegram group/forum の許可 chat ID。 | `hermes gateway setup telegram` / `hermes config set telegram.allowed_chats ...` | config 側の `telegram.allowed_chats` と同概念。 |
| `MATTERMOST_URL` / `MATTERMOST_TOKEN` | 公開 docs + ソース | URL / 文字列 | Mattermost gateway 接続先と認証。 | `hermes gateway setup mattermost` | `mattermost.*` キーの前提になる。 |
| `MATRIX_HOMESERVER` / `MATRIX_ACCESS_TOKEN` / `MATRIX_HOME_ROOM` | 公開 docs + ソース | URL / 文字列 | Matrix gateway 接続先・認証・既定配送先。 | `hermes gateway setup matrix` | `matrix.*` キーの前提になる。 |
| `WHATSAPP_ENABLED` / `WHATSAPP_MODE` / `WHATSAPP_ALLOWED_USERS` / `WHATSAPP_ALLOW_ALL_USERS` / `WHATSAPP_DEBUG` / `WHATSAPP_HOME_CHANNEL` | 公開 docs + ソース | 真偽値 / 文字列 / csv | WhatsApp bridge の有効化と接続モード、許可対象、配送先。 | `hermes gateway setup whatsapp` / `hermes whatsapp` | 現行実装では env 依存が強い。 |

## H-3. プロセス上書きと handoff

| env / 引数 | 根拠 | 設定可能な値 | 役割 | 関連 CLI | 備考 |
| --- | --- | --- | --- | --- | --- |
| `HERMES_IGNORE_USER_CONFIG` | 公開 docs + ソース | `0` / `1` | user config を無視し、built-in defaults と project-level config だけで起動する。 | `--ignore-user-config` | credentials を入れた `.env` は引き続き読む。 |
| `HERMES_IGNORE_RULES` | 公開 docs + ソース | `0` / `1` | `AGENTS.md` `SOUL.md` `.cursorrules` memory preloading をまとめて無視する。 | `--ignore-rules` | 再現テストや隔離実行向け。 |
| `HERMES_YOLO_MODE` | 公開 docs + ソース | `0` / `1` | dangerous command 承認をすべて飛ばす。 | `--yolo` | `approvals.mode: off` と近いが、process-level override。 |
| `HERMES_TUI` | 公開 docs + ソース | `0` / `1` | classic CLI ではなく TUI を起動する。 | `--tui` | TUI handoff 用の追加 env は下の行を参照。 |
| `HERMES_MODEL` | 公開 docs + ソース | model id | process 単位の model override。cron scheduler でも参照される。 | `--model` | 通常運用では `config.yaml` 優先。 |
| `HERMES_INFERENCE_PROVIDER` | 公開 docs + ソース | provider 名 | process 単位の provider override。 | `--provider` | provider 選択の最終上書き。 |
| `HERMES_MAX_ITERATIONS` | 公開 docs + ソース | 正の整数 | process 単位の max turns override。 | `--max-turns` | gateway / cron は `agent.max_turns` を env へ bridge する。 |
| `HERMES_ACCEPT_HOOKS` | ソース | `0` / `1` | 新規 shell hook を TTY なしで自動承認する。 | `--accept-hooks` | gateway / cron の初回 hook 反映で重要。 |
| `HERMES_TUI_RESUME` | 公開 docs + ソース | session id | 指定 session へ TUI 再接続。 | 直接 CLI flag なし | dashboard / sidecar handoff でも使う。 |
| `HERMES_TUI_THEME` | 公開 docs + ソース | `light` / `dark` / 6 桁 hex | TUI の背景テーマ強制。 | 直接 CLI flag なし | 端末の自動検出を上書き。 |
| `HERMES_TUI_DIR` | 公開 docs + ソース | パス | prebuilt `ui-tui/` を使う。 | `--tui` と併用 | distro / Nix 配布向け。 |
| `HERMES_TUI_TOOLSETS` | ソース | csv / `all` | TUI 起動時の有効 toolsets を handoff する。 | TUI 起動内部で利用 | 通常ユーザーが手で触るより起動側が設定。 |
| `HERMES_TUI_SKILLS` | ソース | csv | TUI 起動時の preload skills。 | TUI 起動内部で利用 | 通常は `--skills` から設定される。 |
| `HERMES_TUI_CHECKPOINTS` | ソース | `0` / `1` | TUI 起動時に checkpoints を有効化する。 | `--checkpoints` | classic CLI flag から TUI に橋渡しされる。 |
| `HERMES_TUI_PASS_SESSION_ID` | ソース | `0` / `1` | TUI 起動時に session ID 注入を有効化する。 | `--pass-session-id` | classic CLI flag の handoff。 |
| `HERMES_TUI_MAX_TURNS` | ソース | 正の整数 | TUI 側 max turns handoff。 | `--max-turns` | classic CLI flag の handoff。 |
| `HERMES_TUI_PROVIDER` | ソース | provider 名 | TUI 内で明示 provider を保持する。 | `--provider` | classic CLI flag の handoff。 |
| `HERMES_TUI_QUERY` | ソース | 文字列 | 起動直後の one-shot query。 | `-q` / `--query` | TUI 起動時 handoff。 |
| `HERMES_TUI_IMAGE` | ソース | パス | 起動直後の画像入力 handoff。 | `--image` | TUI 起動時 handoff。 |
| `TERMINAL_LOCAL_PERSISTENT` | 公開 docs + ソース | `0` / `1` | local backend でも persistent shell を使う。 | 直接 CLI flag なし | `terminal.persistent_shell` は remote backend 中心で、local はこの env が別軸。 |
| `HERMES_FILE_MUTATION_VERIFIER` | docs + ソース | `0` / `1` | file mutation verifier footer を process 単位で上書きする。 | 直接 CLI flag なし | `display.file_mutation_verifier` の env override。 |
| `HERMES_LANGUAGE` | docs | UI language code | static UI language を process 単位で上書きする。 | 直接 CLI flag なし | `display.language` より強い。 |

## 読み方

- provider の API key や base URL を調べたいときは H-1 を見る。
- Slack / Discord / Telegram / WhatsApp など gateway 起動前提の token / 接続先は H-2 を見る。
- `--model` `--provider` `--ignore-user-config` のような、その回だけ効く override は H-3 を見る。
