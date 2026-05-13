# Manuscript Structure

## 目的

- KDP 向けに、前付・本文・付録・後付を含めた全体構成を固定する
- 執筆用ファイル群と、組版用原稿の並びを対応づける
- 最終整形時に「どの要素が不足しているか」を見失わないようにする

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

1. 1章 `TypeScript を実務で使う意味`
2. 2章 `型システムの土台`
3. 3章 `関数、オブジェクト、ジェネリクスの設計`
4. 4章 `ランタイム境界とバリデーション`
5. 5章 `モジュール、依存関係、公開 API 設計`
6. 6章 `Node.js アプリケーションでの実践`
7. 7章 `フロントエンドでの実践`
8. 8章 `テスト、ビルド、デバッグ`
9. 9章 `tsconfig と開発体験の設計`
10. 10章 `TypeScript 6.x / 7.x とツールチェーン移行`
11. 11章 `継続運用で壊れない TypeScript`

### 付録

1. 付録 A `TaskHub の全体構成`
2. 付録 B `TaskHub のコードマップ`
3. 付録 C `TaskHub の代表断片`

### 後付

1. おわりに
2. 参考情報
3. 著者紹介
4. 奥付

## ファイル対応

### 本文

- 1章: [01-why-typescript-in-practice.md](/Users/goura32/techbook/books/typescript-practical/chapters/01-why-typescript-in-practice.md)
- 2章: [02-foundations-of-the-type-system.md](/Users/goura32/techbook/books/typescript-practical/chapters/02-foundations-of-the-type-system.md)
- 3章: [03-designing-functions-objects-and-generics.md](/Users/goura32/techbook/books/typescript-practical/chapters/03-designing-functions-objects-and-generics.md)
- 4章: [04-runtime-boundaries-and-validation.md](/Users/goura32/techbook/books/typescript-practical/chapters/04-runtime-boundaries-and-validation.md)
- 5章: [05-modules-dependencies-and-public-api.md](/Users/goura32/techbook/books/typescript-practical/chapters/05-modules-dependencies-and-public-api.md)
- 6章: [06-typescript-in-nodejs-applications.md](/Users/goura32/techbook/books/typescript-practical/chapters/06-typescript-in-nodejs-applications.md)
- 7章: [07-typescript-in-frontend-applications.md](/Users/goura32/techbook/books/typescript-practical/chapters/07-typescript-in-frontend-applications.md)
- 8章: [08-testing-building-and-debugging.md](/Users/goura32/techbook/books/typescript-practical/chapters/08-testing-building-and-debugging.md)
- 9章: [09-tsconfig-and-developer-experience.md](/Users/goura32/techbook/books/typescript-practical/chapters/09-tsconfig-and-developer-experience.md)
- 10章: [10-typescript-6x-7x-and-toolchain-migration.md](/Users/goura32/techbook/books/typescript-practical/chapters/10-typescript-6x-7x-and-toolchain-migration.md)
- 11章: [11-keeping-typescript-maintainable.md](/Users/goura32/techbook/books/typescript-practical/chapters/11-keeping-typescript-maintainable.md)

### 付録

- 付録 A: [appendix-taskhub-structure.md](/Users/goura32/techbook/books/typescript-practical/appendix-taskhub-structure.md)
- 付録 B: [appendix-taskhub-code-map.md](/Users/goura32/techbook/books/typescript-practical/appendix-taskhub-code-map.md)
- 付録 C: [appendix-taskhub-core-fragments.md](/Users/goura32/techbook/books/typescript-practical/appendix-taskhub-core-fragments.md)

### 制作支援

- 図表計画: [figures.md](/Users/goura32/techbook/books/typescript-practical/figures.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/typescript-practical/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/typescript-practical/code-samples.md)
- レイアウト確認: [layout-review.md](/Users/goura32/techbook/books/typescript-practical/layout-review.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/typescript-practical/reference-policy.md)

## 章間のつなぎ

- はじめに:
  - 本書の目的
  - 想定読者
  - `TaskHub` を使う理由
  - 読み方の案内
- 本文 1章から4章:
  - 原則編
- 本文 5章から9章:
  - 実装・設定編
- 本文 10章から11章:
  - 更新・運用編
- おわりに:
  - 本書全体の視点を短く回収する
  - 次に読むとよい巻を案内する

## 未着手の要素

- 扉
- 参考情報の URL / 版情報の最終転記
- 目次の最終粒度決定

現時点では、奥付は仮埋め済みで、発行日は 2026年5月13日を仮設定として最終整形を進める前提とする。

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C の参照表記が本文と一致しているか
- 図表番号と図表キャプションが本文順に並ぶか
- 前付と後付を入れたあとでページバランスが崩れないか

詳細な作業チェックは [final-assembly-checklist.md](/Users/goura32/techbook/books/typescript-practical/final-assembly-checklist.md) を使う。

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

転記候補の管理は [reference-sources.md](/Users/goura32/techbook/books/typescript-practical/reference-sources.md) を使う。
