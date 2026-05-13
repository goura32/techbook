# Appendix A. ForgeFlow の全体構成

## この付録の役割

この付録は、本文中で断片的に登場する `ForgeFlow` の全体像をまとめて参照するためのものです。本書では各章で必要な設定や運用断片だけを示しますが、それだけだと読者が「どの profile が何を担当していたか」「どこで memory や cron を分けていたか」を見失いやすくなります。

## `ForgeFlow` の前提

`ForgeFlow` は、小規模な開発チーム向けの Hermes Agent 運用例です。意図的に次の profile を分けます。

- `coder`
- `ops`
- `triage`

それぞれ独立した config、memory、sessions、skills、cron を持ちますが、必要に応じて同じプロジェクトを扱います。

## 想定構成

```text
~/.hermes/
  profiles/
    coder/
      config.yaml
      .env
      SOUL.md
    ops/
      config.yaml
      .env
      SOUL.md
    triage/
      config.yaml
      .env
      SOUL.md
  skills/
  sessions/
```

## 各 profile の責務

### `coder`

- コード変更
- 小さなリファクタリング
- テスト補助

### `ops`

- 依存更新
- 長時間タスク
- cron と gateway 前提の定期運用

### `triage`

- Issue 整理
- changelog 草案
- 情報収集と要約

## 運用前提

- profile は sandbox ではない
- `terminal.cwd` は profile ごとに明示する
- `SOUL.md` は役割を強めるが filesystem 境界は作らない
- approvals と checkpoints を前提に使う

## 付録 B / C との関係

付録 A は `ForgeFlow` の構造と責務を見るためのものです。付録 B では機能と責務の対応を、付録 C では代表的な設定断片と安全運用断片を確認できます。
