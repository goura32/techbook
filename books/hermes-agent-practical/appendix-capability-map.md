# Appendix B. 主要機能と責務の対応

## この付録の役割

Hermes Agent では、Profiles、Skills、Context Files、Memory、MCP、Checkpoints、Gateway、Cron が同時に見えてくるため、似た役割に見えて混乱しやすくなります。この付録では、`何の課題をどの層で解くか` という観点で機能を整理します。

## 機能と役割の対応

| 機能 | 主な役割 | まず効く章 | やりがちな誤解 |
| --- | --- | --- | --- |
| Profiles | 責務、権限、作業範囲を分ける | 7章 | 人格分離そのものが安全性を生む |
| `SOUL.md` | 長く維持したい原則を固定する | 7章 | 毎回変わる依頼も書く |
| Context Files | プロジェクト固有の前提を固定する | 7章 | profile の代わりになる |
| Skills | 再利用したい手順を固定する | 5章 | 権限まで担っている |
| MCP | 外部能力を明示的に足す | 6章 | 多ければ多いほど便利 |
| Memory | 短中期の文脈を持続させる | 8章 | 原則や恒久ルールも全部覚えさせる |
| Checkpoints | エージェント作業中の巻き戻し点を作る | 9章 | Git の代わりになる |
| Gateway | 常駐入口、sessions、messaging を支える | 10章 | CLI の延長線上でそのまま安全 |
| Cron | 定期実行を支える | 10章 | すべての定期作業に agent が必要 |

## どの課題をどの層で解くか

| 課題 | 第一候補 | 第二候補 | コメント |
| --- | --- | --- | --- |
| 毎回同じ依頼文を書くのがつらい | Skills | Context Files | 手順を固定したいなら Skills |
| プロジェクト前提を毎回説明したくない | Context Files | `SOUL.md` | プロジェクト固有なら Context Files |
| 役割ごとに作業を分けたい | Profiles | toolsets | 状態分離は Profiles が主役 |
| 外部サービスへ触りたい | MCP | script-only workflow | 何でも agent へ寄せない |
| 長いセッションをまたいで薄い文脈を持ちたい | Memory | Context Files | 恒久ルールは文書へ残す |
| 書き換え前へ戻したい | Checkpoints | Git | 短期 rollback は Checkpoints |
| 定期監視したい | Cron | script-only scheduler | agent が必要かを先に判断する |

## 似て見えるものの分け方

### Profile と Skill

- Profile
  - 誰が、どこまで触るか
- Skill
  - どういう順番で、何を見るか

### `SOUL.md` と Context Files

- `SOUL.md`
  - 長く維持したい原則
- Context Files
  - プロジェクト固有の可変前提

### Memory と Context Files

- Memory
  - 短中期の状態
- Context Files
  - 明示的に保守する文書

### Checkpoints と Git

- Checkpoints
  - エージェント作業中の短期保険
- Git
  - 長い時間軸の履歴管理

## この付録の使い方

本文を読んでいて「どの層の話だったか」が分からなくなったら、この付録へ戻ると整理しやすくなります。特に 5章から10章は機能が多く見えるので、役割の重なりを感じたらここで一度切り分け直すのがおすすめです。
