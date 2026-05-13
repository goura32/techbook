# Manuscript Structure

## 目的

- KDP 向けに、前付・本文・付録・後付を含めた全体構成を固定する
- 執筆用ファイル群と、組版用原稿の並びを対応づける
- 最終整形時に不足要素を見失わないようにする

## 全体構成

1. 表紙
2. 扉
3. はじめに
4. 目次
5. 本文 1章から11章
6. 付録 A から C
7. おわりに
8. 参考情報
9. 著者紹介
10. 奥付

## 組版用の並び

### 前付

1. 扉
2. はじめに
3. 本書の読み方
4. 目次

### 本文

1. 1章 `Python を実務で使う見取り図`
2. 2章 `Python らしい設計と読みやすさ`
3. 3章 `データ構造と標準ライブラリの使いどころ`
4. 4章 `型ヒントと読みやすい API 設計`
5. 5章 `例外、ログ、設定、CLI`
6. 6章 `テスト、自動化、品質管理`
7. 7章 ``pyproject.toml`、依存関係、配布`
8. 8章 `非同期処理と I/O`
9. 9章 `Python 3.14 と実行環境の変化`
10. 10章 `Web/API 連携と外部サービス境界`
11. 11章 `長く運用する Python コードベース`

### 付録

1. 付録 A `OpsBox の全体構成`
2. 付録 B `OpsBox のコードマップ`
3. 付録 C `OpsBox の代表断片`

### 後付

1. おわりに
2. 参考情報
3. 著者紹介
4. 奥付

## ファイル対応

### 本文

- 1章: [01-python-in-practice.md](/Users/goura32/techbook/books/python-practical/chapters/01-python-in-practice.md)
- 2章: [02-pythonic-design-and-readability.md](/Users/goura32/techbook/books/python-practical/chapters/02-pythonic-design-and-readability.md)
- 3章: [03-standard-library-in-practice.md](/Users/goura32/techbook/books/python-practical/chapters/03-standard-library-in-practice.md)
- 4章: [04-type-hints-and-api-design.md](/Users/goura32/techbook/books/python-practical/chapters/04-type-hints-and-api-design.md)
- 5章: [05-exceptions-logging-config-and-cli.md](/Users/goura32/techbook/books/python-practical/chapters/05-exceptions-logging-config-and-cli.md)
- 6章: [06-testing-automation-and-quality.md](/Users/goura32/techbook/books/python-practical/chapters/06-testing-automation-and-quality.md)
- 7章: [07-pyproject-dependencies-and-distribution.md](/Users/goura32/techbook/books/python-practical/chapters/07-pyproject-dependencies-and-distribution.md)
- 8章: [08-async-io-and-concurrency.md](/Users/goura32/techbook/books/python-practical/chapters/08-async-io-and-concurrency.md)
- 9章: [09-python-314-and-runtime-changes.md](/Users/goura32/techbook/books/python-practical/chapters/09-python-314-and-runtime-changes.md)
- 10章: [10-web-api-boundaries.md](/Users/goura32/techbook/books/python-practical/chapters/10-web-api-boundaries.md)
- 11章: [11-keeping-python-maintainable.md](/Users/goura32/techbook/books/python-practical/chapters/11-keeping-python-maintainable.md)

### 付録

- 付録 A: [appendix-opsbox-structure.md](/Users/goura32/techbook/books/python-practical/appendix-opsbox-structure.md)
- 付録 B: [appendix-opsbox-code-map.md](/Users/goura32/techbook/books/python-practical/appendix-opsbox-code-map.md)
- 付録 C: [appendix-opsbox-core-fragments.md](/Users/goura32/techbook/books/python-practical/appendix-opsbox-core-fragments.md)

### 制作支援

- 図表計画: [figures.md](/Users/goura32/techbook/books/python-practical/figures.md)
- 図表ラフ: [figures-roughs.md](/Users/goura32/techbook/books/python-practical/figures-roughs.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/python-practical/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/python-practical/code-samples.md)
- レイアウト確認: [layout-review.md](/Users/goura32/techbook/books/python-practical/layout-review.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/python-practical/reference-policy.md)

## 章間のつなぎ

- はじめに:
  - 本書の目的
  - 想定読者
  - `OpsBox` を使う理由
  - 読み方の案内
- 本文 1章から5章:
  - 原則編
- 本文 6章から8章:
  - テスト、配布、非同期の実装編
- 本文 9章から11章:
  - 更新、外部境界、継続運用編
- おわりに:
  - 本書全体の視点を短く回収する

## 未着手の要素

- 扉の最終表記
- 参考情報の URL / 版情報の最終転記
- 目次の最終粒度決定

現時点では、奥付は仮埋め済みで、発行日は 2026年5月13日を仮設定として最終整形を進める前提とする。

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C の参照表記が本文と一致しているか
- 図表番号と図表キャプションが本文順に並ぶか
- 前付と後付を入れたあとでページバランスが崩れないか

詳細な作業チェックは [final-assembly-checklist.md](/Users/goura32/techbook/books/python-practical/final-assembly-checklist.md) を使う。

## 目次粒度の採用方針

- 基本は 1 段階目の見出しのみ
- 章数が多いため、まずは一覧性を優先する
- 節見出しを追加する場合は、長い章だけへ限定する
- 付録と後付の主要見出しは目次へ含める

## 参考情報の最終転記方針

- 公式情報を優先する
- 本文で直接使った判断材料を優先する
- 日付依存の強い項目だけ版や公開日を併記する
- URL は組版直前に一括で点検して転記する

転記候補の管理は [reference-sources.md](/Users/goura32/techbook/books/python-practical/reference-sources.md) を使う。
