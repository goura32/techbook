# Book Template

各書籍ディレクトリは、次の構成を基本とします。

```text
books/<book-id>/
  outline.md
  target-reader.md
  writing-policy.md
  status.md
  chapters/
```

## `outline.md`

- 書籍の目的
- 章一覧
- 各章の要点
- 他書との役割分担メモ

## `target-reader.md`

- 想定読者
- 前提知識
- 読了後にできるようになってほしいこと
- 対象外とする範囲

## `writing-policy.md`

- この書籍で特に重視する観点
- コード例の前提環境
- 他書との棲み分け
- 共通方針に対する例外

## `status.md`

- 全体ステータス
- 章ごとの状態
- 未解決課題
- 次に着手すること

## `chapters/`

- 章本文ファイルを配置する
- ファイル名は順序が分かるよう `01-...md` の形式を推奨する
- 付録は `appendix-...md` のように区別できる名前にする

## 共通原稿

- 著者紹介は、原則として [author-bio.md](/Users/goura32/techbook/shared/author-bio.md) を土台にする
