# 付録 G. 互換キー・動的キー完全表

## この付録の役割

この付録は、`DEFAULT_CONFIG` の単純な leaf 一覧には現れないが、公開 docs または最新ソースコードで現役として扱われているキーをまとめたものです。

- 対象: 旧互換キー、動的マップ、MCP server 定義、named provider / custom provider 定義、OpenRouter routing、SSH など backend 個別キー
- 根拠: `cli-config.yaml.example`、`website/docs/user-guide/configuration.md`、`website/docs/reference/mcp-config-reference.md`、`hermes_cli/config.py`、`cli.py`、`gateway/run.py`

| キー | 設定可能な値 | 既定値 | 関連 env | 関連 CLI / コマンド | 備考 |
| --- | --- | --- | --- | --- | --- |
| `model.default` | `provider/model` 形式の文字列 | `""` | `HERMES_MODEL` | `hermes chat --model` / `hermes model` | 旧来の dict 形式。root の `model:` 文字列より冗長だが現行ソースでも受理される。 |
| `model.provider` | provider 名 | `auto` | `HERMES_INFERENCE_PROVIDER` | `hermes chat --provider` / `hermes model` | root `model` と組み合わせる旧来の明示キー。 |
| `model.base_url` | OpenAI-compatible endpoint URL | `""` | `OPENROUTER_BASE_URL` など provider 固有 base URL env | `hermes model` | provider ごとの既定 URL を上書きするときに使う。 |
| `model.context_length` | 正の整数 | 自動検出 / provider metadata | — | — | ソースは `gateway/run.py` と `agent.model_metadata`。自動検出が誤る local / proxy 環境でのみ明示。 |
| `model.max_tokens` | 正の整数 | モデル固有上限 | — | — | 出力トークン上限。context_length と混同しやすい。 |
| `providers.<id>.request_timeout_seconds` | 秒数 | 未設定時は既定 timeout | — | `hermes config set` | provider-wide timeout。docs と `cli-config.yaml.example` にあり。 |
| `providers.<id>.stale_timeout_seconds` | 秒数 | 未設定時は既定 stale timeout | — | `hermes config set` | non-stream stale detector 用。 |
| `providers.<id>.models.<model>.timeout_seconds` | 秒数 | provider 既定を継承 | — | `hermes config set` | model 単位 override。 |
| `providers.<id>.models.<model>.stale_timeout_seconds` | 秒数 | provider 既定を継承 | — | `hermes config set` | model 単位 stale detector override。 |
| `provider_routing.sort` | `price` / `throughput` / `latency` | provider 側既定 | — | — | OpenRouter へ転送する routing preference。 |
| `provider_routing.only` | provider slug 配列 | 未設定 | — | — | OpenRouter の利用 provider を許可リスト化。 |
| `provider_routing.ignore` | provider slug 配列 | 未設定 | — | — | OpenRouter の利用 provider を除外。 |
| `provider_routing.order` | provider slug 配列 | 未設定 | — | — | OpenRouter の試行順を固定。 |
| `provider_routing.require_parameters` | `true` / `false` | `false` 相当 | — | — | 要求パラメータの完全対応を provider へ要求する。 |
| `provider_routing.data_collection` | `allow` / `deny` | `allow` | — | — | データ保存を許す provider だけ / 禁じる provider だけに絞る。 |
| `fallback_model` | 1 個の dict または dict 配列 | 未設定 | — | `hermes fallback` | 旧互換。新規構成は `fallback_providers` を優先。 |
| `custom_providers` | provider 定義の YAML 配列 | 未設定 | — | `hermes model` / `hermes config set` | 旧互換。現行は `providers` マップへ正規化される。 |
| `custom_providers[].name` | 識別子 | 必須 | — | — | custom provider 名。 |
| `custom_providers[].base_url` | endpoint URL | 必須 | `OPENAI_BASE_URL` 系を直接使う場合もある | — | OpenAI-compatible endpoint。 |
| `custom_providers[].api_key` | 文字列 / `${ENV}` | 未設定 | `OPENAI_API_KEY` など | — | secret は通常 `.env` へ逃がす。 |
| `custom_providers[].models[].id` | model id | 未設定 | — | — | display 用の curated model 一覧。 |
| `custom_providers[].models[].context_length` | 正の整数 | 未設定 | — | — | local / proxy model の context_length 明示 override。 |
| `mcp_servers.<name>.command` | 実行ファイル名 | 未設定 | — | `hermes mcp add --command` | stdio MCP server 用。 |
| `mcp_servers.<name>.args` | 文字列配列 | 空配列 | — | `hermes mcp add --args ...` | stdio MCP server 引数。 |
| `mcp_servers.<name>.env` | 環境変数マップ | 空マップ | server 個別の API key env | `hermes mcp add --env KEY=VALUE` | stdio MCP subprocess へ渡す。 |
| `mcp_servers.<name>.url` | HTTP/SSE URL | 未設定 | — | `hermes mcp add --url` | remote MCP server 用。 |
| `mcp_servers.<name>.headers` | HTTP header マップ | 空マップ | — | `hermes mcp configure` | header auth など。 |
| `mcp_servers.<name>.enabled` | `true` / `false` | `true` | — | `hermes mcp configure` | 接続を一時停止するときに使う。 |
| `mcp_servers.<name>.timeout` | 秒数 | `120` | — | `hermes mcp configure` | tool call timeout。 |
| `mcp_servers.<name>.connect_timeout` | 秒数 | `60` | — | `hermes mcp configure` | 初回接続 timeout。 |
| `mcp_servers.<name>.tools.include` | tool 名または配列 | 未設定 | — | `hermes mcp configure` | allowlist。 |
| `mcp_servers.<name>.tools.exclude` | tool 名または配列 | 未設定 | — | `hermes mcp configure` | denylist。 |
| `mcp_servers.<name>.tools.resources` | `true` / `false` | `true` 相当 | — | `hermes mcp configure` | resource utility tools を有効化。 |
| `mcp_servers.<name>.tools.prompts` | `true` / `false` | `true` 相当 | — | `hermes mcp configure` | prompt utility tools を有効化。 |
| `mcp_servers.<name>.auth` | `oauth` / `header` | 未設定 | — | `hermes mcp add --auth` | OAuth 2.1 PKCE か header auth。 |
| `mcp_servers.<name>.sampling` | マップ | 未設定 | — | `hermes mcp configure` | server-initiated LLM request policy。 |
| `platform_toolsets` | platform ごとの有効 toolset / MCP server マップ | 未設定 | — | `hermes tools` | 実装管理キー。手編集より `hermes tools` を推奨。 |
| `terminal.lifetime_seconds` | 秒数 | `300` | `TERMINAL_LIFETIME_SECONDS` | `hermes config set terminal.lifetime_seconds ...` | 長時間 terminal session の寿命。 |
| `terminal.ssh_host` | ホスト名 | 未設定 | `TERMINAL_SSH_HOST` | `hermes setup terminal` | SSH backend 用。 |
| `terminal.ssh_user` | ユーザー名 | 未設定 | `TERMINAL_SSH_USER` | `hermes setup terminal` | SSH backend 用。 |
| `terminal.ssh_port` | 整数 | `22` | `TERMINAL_SSH_PORT` | `hermes setup terminal` | SSH backend 用。 |
| `terminal.ssh_key` | 鍵ファイルパス | 未設定 | `TERMINAL_SSH_KEY` | `hermes setup terminal` | SSH backend 用。 |
| `terminal.docker_env` | key/value マップ | 空マップ | `TERMINAL_DOCKER_ENV` | `hermes config set terminal.docker_env ...` | host 環境を読まずに Docker へ明示 env を渡す。 |
| `terminal.sandbox_dir` | ホスト側パス | backend 実装既定 | `TERMINAL_SANDBOX_DIR` | `hermes config set terminal.sandbox_dir ...` | container backend の workspace / overlay 置き場。 |
