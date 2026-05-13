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

1. 1章 `USB を仕様から理解するための見取り図`
2. 2章 `バス、トポロジ、ホストとデバイスの役割`
3. 3章 `列挙、descriptor、USB 2.0 仕様 Chapter 9 の基本`
4. 4章 `転送方式とスケジューリング`
5. 5章 `コネクタ、ケーブル、USB Type-C`
6. 6章 `電力供給、USB PD、役割交渉`
7. 7章 `USB 3.2、USB4、その前後関係`
8. 8章 `デバイスクラス、ドライバ、OS の見え方`
9. 9章 `TraceDock` を実装視点で読む
10. 10章 `解析、テスト、コンプライアンス`
11. 11章 `長く保守できる USB 製品設計`

### 付録

1. 付録 A `TraceDock の全体構成`
2. 付録 B `descriptor と class の対応`
3. 付録 C `capture、dump、debug の代表断片`

### 後付

1. おわりに
2. 参考情報
3. 著者紹介
4. 奥付

## ファイル対応

### 本文

- 1章: [01-why-usb-from-spec.md](/Users/goura32/techbook/books/usb-from-spec/chapters/01-why-usb-from-spec.md)
- 2章: [02-bus-topology-and-roles.md](/Users/goura32/techbook/books/usb-from-spec/chapters/02-bus-topology-and-roles.md)
- 3章: [03-enumeration-and-descriptors.md](/Users/goura32/techbook/books/usb-from-spec/chapters/03-enumeration-and-descriptors.md)
- 4章: [04-transfer-types-and-scheduling.md](/Users/goura32/techbook/books/usb-from-spec/chapters/04-transfer-types-and-scheduling.md)
- 5章: [05-connectors-cables-and-type-c.md](/Users/goura32/techbook/books/usb-from-spec/chapters/05-connectors-cables-and-type-c.md)
- 6章: [06-power-delivery-and-power-rules.md](/Users/goura32/techbook/books/usb-from-spec/chapters/06-power-delivery-and-power-rules.md)
- 7章: [07-usb-32-usb4-and-generation-gaps.md](/Users/goura32/techbook/books/usb-from-spec/chapters/07-usb-32-usb4-and-generation-gaps.md)
- 8章: [08-device-classes-drivers-and-os-behavior.md](/Users/goura32/techbook/books/usb-from-spec/chapters/08-device-classes-drivers-and-os-behavior.md)
- 9章: [09-implementing-tracedock.md](/Users/goura32/techbook/books/usb-from-spec/chapters/09-implementing-tracedock.md)
- 10章: [10-debugging-testing-and-compliance.md](/Users/goura32/techbook/books/usb-from-spec/chapters/10-debugging-testing-and-compliance.md)
- 11章: [11-keeping-usb-products-maintainable.md](/Users/goura32/techbook/books/usb-from-spec/chapters/11-keeping-usb-products-maintainable.md)

### 付録

- 付録 A: [appendix-tracedock-structure.md](/Users/goura32/techbook/books/usb-from-spec/appendix-tracedock-structure.md)
- 付録 B: [appendix-descriptor-map.md](/Users/goura32/techbook/books/usb-from-spec/appendix-descriptor-map.md)
- 付録 C: [appendix-capture-and-debug-fragments.md](/Users/goura32/techbook/books/usb-from-spec/appendix-capture-and-debug-fragments.md)

### 制作支援

- 図表計画: [figures.md](/Users/goura32/techbook/books/usb-from-spec/figures.md)
- 図表ラフ: [figures-roughs.md](/Users/goura32/techbook/books/usb-from-spec/figures-roughs.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/usb-from-spec/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/usb-from-spec/code-samples.md)
- レイアウト確認: [layout-review.md](/Users/goura32/techbook/books/usb-from-spec/layout-review.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/usb-from-spec/reference-policy.md)

## 章間のつなぎ

- はじめに:
  - 本書の目的
  - 想定読者
  - `TraceDock` を使う理由
  - 読み方の案内
- 本文 1章から4章:
  - USB の土台編
- 本文 5章から8章:
  - Type-C、PD、世代、class の構造編
- 本文 9章から11章:
  - 実装、解析、長期保守編
- おわりに:
  - 本書全体の視点を短く回収する

## 固定済みの前提

- 扉、前付、後付の構成は固定済み
- 目次は章見出し単位を基本にする
- 参考情報は公式情報優先で転記する
- 発行日は 2026年5月13日を仮設定として保持する

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C の参照表記が本文と一致しているか
- 図表番号と図表キャプションが本文順に並ぶか
- 前付と後付を入れたあとでページバランスが崩れないか

詳細な作業チェックは [final-assembly-checklist.md](/Users/goura32/techbook/books/usb-from-spec/final-assembly-checklist.md) を使う。

## 目次粒度の採用方針

- 基本は 1 段階目の見出しのみ
- 規格系の本なので、まずは一覧性を優先する
- 節見出しを追加する場合は、enumeration や PD のような長い章だけへ限定する
- 付録と後付の主要見出しは目次へ含める

## 参考情報の最終転記方針

- 公式情報を優先する
- 本文で直接使った判断材料を優先する
- 日付依存の強い項目だけ公開日を併記する
- URL は組版直前に一括で点検して転記する

転記候補の管理は [reference-sources.md](/Users/goura32/techbook/books/usb-from-spec/reference-sources.md) を使う。
