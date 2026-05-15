# Appendix A. ForgeFlow の全体構成

## この付録の役割

本文では `ForgeFlow` を、Hermes Agent の運用設計を考えるための通しサンプルとして使います。この付録では、各章で断片的に登場する `coder` `triage` `ops` の関係、作業範囲、能力追加の順番をまとめて確認できるようにします。

## `ForgeFlow` の前提

`ForgeFlow` は、小規模な開発チーム向けに Hermes Agent を段階的に導入する想定例です。最初から全部を有効化するのではなく、次の順番で増やします。

1. `coder` を CLI で小さく動かす
2. よく使う手順を Skill 化する
3. 必要な外部能力だけを MCP で足す
4. `triage` と `ops` を分離する
5. 必要になったら Memory、Gateway、Cron を追加する

## profile ごとの責務

| profile | 主な仕事 | 触るもの | 触らないもの |
| --- | --- | --- | --- |
| `coder` | コード修正、テスト補助、差分確認 | リポジトリ、テスト結果、限定された shell | 通知先、広い運用設定 |
| `triage` | issue 整理、要約、優先度候補 | issue、PR、changelog 素材 | コード修正、定期実行 |
| `ops` | 監視、通知、定期運用 | cron、gateway、監視対象、通知先 | 広いコード変更 |

## 想定ディレクトリ構成

```text
~/.hermes/
  config.yaml
  .env
  profiles/
    coder/
      SOUL.md
      context/
    triage/
      SOUL.md
      context/
    ops/
      SOUL.md
      context/
  skills/
  memories/
  cron/
  sessions/
  logs/
```

実際の profile 管理方式やファイル配置は環境によって変わりますが、この付録で見たいのは「責務がどこで分かれるか」です。profile を増やしても、責務境界が見えないなら運用は強くなりません。

## 能力追加の順番

| 段階 | 追加するもの | 追加しないもの | 理由 |
| --- | --- | --- | --- |
| 導入直後 | CLI、最小 config、最初の profile | MCP、Cron、Gateway | 原因切り分けを単純に保つ |
| 安定化 | Skills、Context Files | 常駐運用 | 成功した手順を固定する |
| 拡張 | MCP、必要最小限の Memory | 多数の provider 切替 | 外部能力を明示的に足す |
| 常駐化 | Gateway、Cron、notifications | 無制限の自動変更 | 監視と停止条件を先に作る |

## この付録の見どころ

本文を読み進めていると、`coder` に何を任せていたか、`ops` に何を分けていたかを見失いやすくなります。そのときはこの付録へ戻り、責務の切り方を確認してください。設定例そのものは付録 C、機能ごとの使い分けは付録 B、設定項目や既定値は付録 D〜F、互換キーや process-level override は付録 G/H が向いています。
