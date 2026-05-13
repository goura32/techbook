# Appendix C. 代表的な設定断片と安全運用断片

## この付録の役割

付録 A は `ForgeFlow` の構造、付録 B は機能と責務の対応を整理するためのものでした。この付録 C では、本文で断片的に登場した設定や安全運用の例を、もう少し連続した形で確認できるようにします。

## 1. profile を作る

```bash
hermes profile create coder
coder setup
coder chat
```

ここで大事なのは、profile を「状態の分離」として使うことです。sandbox と同義ではありません。

## 2. `terminal.cwd` を明示する

```yaml
terminal:
  backend: local
  cwd: /absolute/path/to/project
```

既定の起動場所に期待しすぎず、どこでコマンドが始まるかを明示したほうが安全です。

## 3. MCP サーバーを足す

```yaml
mcp_servers:
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "***"
```

MCP は能力を広げますが、同時に権限も広げます。どの server を見せるかは明示的に決める必要があります。

## 4. memory provider を設定する

```yaml
memory:
  provider: openviking
```

built-in memory を置き換えるのではなく、加算で使われる点に注意します。

## 5. cron を作る

```bash
hermes cron create "every 2h" "Check stalled issues" --skill triage
```

cron は gateway 前提で動きます。定期運用に広げる前に、監視と停止方法を決めておいたほうが安全です。

## 6. rollback を前提に進める

重要なのは、agent がファイル変更を行う前に checkpoints が有効か確認することです。問題が起きたときに `rollback` できる前提があるだけで、運用の心理的安全性が大きく変わります。
