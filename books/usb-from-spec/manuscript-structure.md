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
5. 本文 1章から12章
6. 付録 A から D
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

1. 1章 `USB を規格として読むための見取り図`
2. 2章 `USB 2.0 の基本モデル`
3. 3章 `列挙と USB 2.0 仕様のデバイスフレームワーク`
4. 4章 `descriptor の読み方と host の判断`
5. 5章 `transfer type と scheduling`
6. 6章 `device class、driver、OS の見え方`
7. 7章 `USB 3.2 と高速側の論点`
8. 8章 `USB Type-C の配線、CC、役割、ケーブル`
9. 9章 `USB Power Delivery と PD コントローラの実務`
10. 10章 `USB4、Thunderbolt 互換、DisplayPort Alt Mode`
11. 11章 `観測手法、試験、コンプライアンス`
12. 12章 `実装・解析・長期保守`

### 付録

1. 付録 A `descriptor dump の読み方`
2. 付録 B `観測ログの読み方`
3. 付録 C `USB Type-C / PD の最小ハード前提`
4. 付録 D `HID と USB ゲームコントローラー`

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
- 4章: [04-reading-descriptors-and-host-decisions.md](/Users/goura32/techbook/books/usb-from-spec/chapters/04-reading-descriptors-and-host-decisions.md)
- 5章: [05-transfer-types-and-scheduling.md](/Users/goura32/techbook/books/usb-from-spec/chapters/05-transfer-types-and-scheduling.md)
- 6章: [06-device-classes-drivers-and-os-behavior.md](/Users/goura32/techbook/books/usb-from-spec/chapters/06-device-classes-drivers-and-os-behavior.md)
- 7章: [07-usb-32-and-high-speed-considerations.md](/Users/goura32/techbook/books/usb-from-spec/chapters/07-usb-32-and-high-speed-considerations.md)
- 8章: [08-usb-type-c-roles-cables-and-cc.md](/Users/goura32/techbook/books/usb-from-spec/chapters/08-usb-type-c-roles-cables-and-cc.md)
- 9章: [09-usb-power-delivery-and-pd-controllers.md](/Users/goura32/techbook/books/usb-from-spec/chapters/09-usb-power-delivery-and-pd-controllers.md)
- 10章: [10-usb4-thunderbolt-and-displayport-alt-mode.md](/Users/goura32/techbook/books/usb-from-spec/chapters/10-usb4-thunderbolt-and-displayport-alt-mode.md)
- 11章: [11-observation-testing-and-compliance.md](/Users/goura32/techbook/books/usb-from-spec/chapters/11-observation-testing-and-compliance.md)
- 12章: [12-implementation-analysis-and-long-term-maintenance.md](/Users/goura32/techbook/books/usb-from-spec/chapters/12-implementation-analysis-and-long-term-maintenance.md)

### 付録

- 付録 A: [appendix-descriptor-map.md](/Users/goura32/techbook/books/usb-from-spec/appendix-descriptor-map.md)
- 付録 B: [appendix-capture-and-debug-fragments.md](/Users/goura32/techbook/books/usb-from-spec/appendix-capture-and-debug-fragments.md)
- 付録 C: [appendix-typec-pd-hardware-basics.md](/Users/goura32/techbook/books/usb-from-spec/appendix-typec-pd-hardware-basics.md)
- 付録 D: [appendix-hid-game-controllers.md](/Users/goura32/techbook/books/usb-from-spec/appendix-hid-game-controllers.md)

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
  - 仕様、観測、ハード前提の案内
  - 読み方の案内
- 本文 1章から5章:
  - USB 2.0 の土台編
- 本文 6章から10章:
  - class、USB 3.2、Type-C、PD、USB4 の構造編
- 本文 11章から12章:
  - 観測、試験、長期保守編
- おわりに:
  - 本書全体の視点を短く回収する

## 固定済みの前提

- 扉、前付、後付の構成は固定済み
- 目次は章見出し単位を基本にする
- 参考情報は公式情報優先で転記する
- 発行日は 2026年5月13日を仮設定として保持する

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C / D の参照表記が本文と一致しているか
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
