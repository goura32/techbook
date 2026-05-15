# Appendix C. 設定断片と安全運用断片

## この付録の役割

本文では判断軸を重視しました。この付録では、`最小構成` `MCP 追加` `profile ごとの作業範囲` `cron の最小例` のように、実務で見返しやすい断片を置きます。完全な設定項目の一覧は付録 D〜F、互換キーと動的キーは付録 G、認証系環境変数とプロセス単位の env / CLI override は付録 H を参照してください。

## C-1. 最小構成の `config.yaml`

```yaml
model: anthropic/claude-sonnet-4

terminal:
  backend: local
  cwd: /absolute/path/to/project
  timeout: 180
```

この段階では、重要なのは豪華な構成ではありません。どの provider へ出ていて、どこで shell が動き、どの profile がこの設定を読むのかを説明できることです。

## C-2. provider と `.env` の分離

```env
ANTHROPIC_API_KEY=...
GITHUB_TOKEN=...
```

```yaml
model: anthropic/claude-sonnet-4

providers:
  anthropic:
    request_timeout_seconds: 120
```

ポイントは、secret と非secret を分けることです。API キーを `config.yaml` に混ぜないだけで、差し替えと監査がかなり楽になります。

## C-3. `terminal.cwd` を固定する

```yaml
terminal:
  backend: docker
  cwd: /workspace/project
  timeout: 180
```

`terminal.cwd` は、CLI、Gateway、Cron の認識差を生みやすい項目です。意図した場所を向いていることを早い段階で確認しておくと、あとで長時間運用へ広げても壊れにくくなります。

## C-4. MCP サーバーを 1 台だけ足す

```yaml
mcp_servers:
  github:
    command: npx
    args:
      - -y
      - "@modelcontextprotocol/server-github"
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: ${GITHUB_TOKEN}
```

ここで見たいのは、MCP が足せることではありません。server の構成は `config.yaml`、token は `.env` へ分け、server を 1 台ずつ増やすことです。複数台を一気に入れると、障害時の切り分けが急に難しくなります。

## C-5. `coder` と `ops` の責務を分ける

`coder` には、コード変更とテスト補助だけを与えます。`ops` には、通知、監視、定期実行だけを与えます。これを設定ファイルだけで完全に実現できるわけではありませんが、profile、toolset、approval、実行経路を揃えて設計すると、責務境界がかなり明確になります。

この付録では断片だけを示しますが、具体的に見たいのは次です。

- `coder`
  - ファイル変更前提
  - Checkpoints を使う
- `ops`
  - Cron / Gateway 前提
  - 外部投稿を伴う
  - 広いコード修正はさせない

## C-6. cron の最小例

```bash
hermes cron create "every 2h" "Check stale issues" --skill triage
```

この種のジョブで重要なのは、実行できることではなく、止められることです。頻度、通知先、失敗回数の閾値、人間へ戻す条件がないジョブは、便利さより運用負債のほうが大きくなります。

## C-7. rollback 前提で進める

ファイル変更を伴う仕事では、Checkpoints を前提に進めるだけで心理的安全性がかなり変わります。ただし `rollback` できることと、安全に復旧できることは別です。戻したあとに何を確認するかまで含めて運用手順にしておく必要があります。

この付録の断片は、本文 2章から10章を読んだあとに見返すと使いやすくなります。より広い設定項目の一覧は付録 D〜F、互換キーや動的キーの確認は付録 G、認証系環境変数や起動時 override の確認は付録 H が向いています。
