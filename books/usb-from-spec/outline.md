# Outline

## 書籍の目的

- USB を単なるコネクタ名や速度表記ではなく、`USB 2.0` `USB 3.2` `USB Type-C` `USB Power Delivery` `USB4` `Alt Mode` `Thunderbolt 互換` が重なる体系として理解できるようにする
- 規格書を読んでも実装や解析に結びつかない、という壁を越えるために、`仕様` `観測` `最低限のハード前提` をひとつの本にまとめる
- `つながらない` `映像が出ない` `高機能モードに入らない` `host により挙動が違う` といった現場の症状を、どの層で切り分けるべきか判断できるようにする

## 仮タイトル

- 技術の輪郭 USB
- サブタイトル案: 規格、Type-C、PD、USB4、観測までを実務でつなぐ

## 本書の軸

- 主役は USB 規格そのもの
- 通しサンプルは補助にとどめる
- 特定の架空デバイスは本文の中心に置かず、必要なら短いケーススタディへとどめる
- 深掘りの中心は `USB-IF` `VESA` `Wireshark / usbmon / USBPcap` の一次情報と観測手法に置く

## 章構成案

1. USB を規格として読むための見取り図
2. USB 2.0 の基本モデル
3. 列挙と USB 2.0 仕様のデバイスフレームワーク
4. descriptor の読み方と host の判断
5. transfer type と scheduling
6. device class、driver、OS の見え方
7. USB 3.2 と高速側の論点
8. USB Type-C の配線、CC、役割、ケーブル
9. USB Power Delivery と PD コントローラの実務
10. USB4、Thunderbolt 互換、DisplayPort Alt Mode
11. 観測手法、試験、コンプライアンス
12. 実装・解析・長期保守

## 詳細構成

### 1. USB を規格として読むための見取り図

- 1-1. USB をコネクタ名や marketing 名で覚えない
- 1-2. データ、電力、役割、世代、映像、互換の話を分ける
- 1-3. `仕様` `観測` `ハード前提` の 3 本柱
- 1-4. 本書で扱う範囲と扱わない範囲
- 1-5. どの規格書をどの目的で読むか

### 2. USB 2.0 の基本モデル

- 2-1. host、hub、device、interface、endpoint
- 2-2. host 主導という前提
- 2-3. port、address、configuration の役割
- 2-4. bus power と default power
- 2-5. 何が物理層で、何が論理層か

### 3. 列挙と USB 2.0 仕様のデバイスフレームワーク

- 3-0. USB 2.0 規格書第9章とは何か
- 3-1. attach から reset まで
- 3-2. GET_DESCRIPTOR と SET_ADDRESS
- 3-3. setup / data / status stage
- 3-4. どこで止まると何を疑うか
- 3-5. analyzer や dump で見るべき最小単位
- 3-6. ケーススタディ: 列挙が途中で崩れるとき

### 4. descriptor の読み方と host の判断

- 4-1. device / configuration / interface / endpoint descriptor
- 4-2. class を device に置くか interface に置くか
- 4-3. string descriptor と識別情報
- 4-4. 電力値、interface 数、endpoint 定義の見方
- 4-5. descriptor の崩れが driver 問題に見える場面
- 4-6. descriptor dump を行単位で読む

### 5. transfer type と scheduling

- 5-1. control / bulk / interrupt / isochronous の契約差
- 5-2. 帯域、遅延、再送、完全性
- 5-3. polling と host 側 scheduling
- 5-4. 何をどの transfer に載せるべきか
- 5-5. transfer の選び方が debug に与える影響
- 5-6. ケーススタディ: bulk は遅いが壊れてはいない

### 6. device class、driver、OS の見え方

- 6-1. class / subclass / protocol の役割
- 6-2. HID、CDC、MSC、vendor-specific
- 6-3. Windows / macOS / Linux での見え方差
- 6-4. class 準拠と独自実装の分岐
- 6-5. driver 問題と descriptor 問題をどう分けるか
- 6-6. host 側サポートコストの見積もり

### 7. USB 3.2 と高速側の論点

- 7-1. USB 2.0 と USB 3.x の重なり方
- 7-2. Gen1 / Gen2 / lane の考え方
- 7-3. cable、hub、host controller が増やす観測点
- 7-4. backwards compatibility の実態
- 7-5. 高速側だけ不安定になるときの見方
- 7-6. marketing 文言と仕様上の能力を分ける

### 8. USB Type-C の配線、CC、役割、ケーブル

- 8-1. USB Type-C connector の信号群
- 8-2. CC1 / CC2、VBUS、VCONN、SBU
- 8-3. Rp / Rd / Ra と attach 判定
- 8-4. source / sink、DFP / UFP、DRP
- 8-5. cable orientation、mux、e-marker
- 8-6. DisplayPort Alt Mode や USB4 の入口としての Type-C

### 9. USB Power Delivery と PD コントローラの実務

- 9-1. Type-C と PD は何が違うか
- 9-2. source capabilities、sink capabilities、PDO
- 9-3. explicit contract と role swap
- 9-4. EPR と高電力化
- 9-5. PD controller / port controller / PMIC の役割
- 9-6. PD チップが返す status、fault、capability 情報
- 9-7. ケーススタディ: attach はするが期待機能へ入らない

### 10. USB4、Thunderbolt 互換、DisplayPort Alt Mode

- 10-1. USB4 は何を追加するか
- 10-2. USB4 discovery と entry
- 10-3. Thunderbolt 3 compatibility の位置づけ
- 10-4. DisplayPort Alt Mode で映像が出る仕組み
- 10-5. 映像、USB、電力が同じ Type-C で競合する場面
- 10-6. dock、hub、cable で結果が変わる理由

### 11. 観測手法、試験、コンプライアンス

- 11-1. usbmon、Wireshark、USBPcap、tshark
- 11-2. descriptor dump と OS 標準ツール
- 11-3. protocol analyzer が必要になる境界
- 11-4. xHCI interop と electrical test の違い
- 11-5. 再現条件をどう残すか
- 11-6. 認証と実装品質を混同しない

### 12. 実装・解析・長期保守

- 12-1. 最小成立構成を先に決める
- 12-2. host 側と device 側の責務分離
- 12-3. cable、OS、power 条件を記録する
- 12-4. 規格更新をどう監視するか
- 12-5. USB を選ばない判断も持つ
- 12-6. 仕様を読めることを現場価値へ変える

## 付録案

### 付録 A. descriptor dump の読み方

- A-1. device descriptor の行ごとの見方
- A-2. configuration tree の追い方
- A-3. class と endpoint の対応をどう確認するか

### 付録 B. 観測ログの読み方

- B-1. setup / data / status stage の追い方
- B-2. Wireshark / usbmon の基本表示
- B-3. 症状から最初に見る evidence

### 付録 C. USB Type-C / PD の最小ハード前提

- C-1. CC1 / CC2、VBUS、VCONN、SBU
- C-2. Rp / Rd / Ra の意味
- C-3. PD controller の status と fault

### 付録 D. HID と USB ゲームコントローラー

- D-1. HID class を USB 規格の中でどう位置づけるか
- D-2. joystick / gamepad の report descriptor の考え方
- D-3. button、axis、hat switch をどう表現するか
- D-4. OS からどう見えるか
- D-5. よくある不具合と切り分け

## 各章の要点

- 1章: USB を読む地図を作る
- 2章: USB 2.0 の土台を固める
- 3章: 列挙と USB 2.0 仕様のデバイスフレームワークを観測可能な形で理解する
- 4章: descriptor を host 判断の契約書として読む
- 5章: transfer を性能表ではなく契約差として理解する
- 6章: class、driver、OS 差分を実務へ引き戻す
- 7章: USB 3.2 と高速側の追加論点を切り分ける
- 8章: Type-C の配線と役割の基礎を押さえる
- 9章: PD と PD controller の実務観測点を理解する
- 10章: USB4、Thunderbolt、Alt Mode を現代 USB の文脈へ置く
- 11章: 観測、試験、コンプライアンスの境界を整理する
- 12章: 実装と保守の原則で閉じる

## 他書との役割分担メモ

- `Git & GitHub` の変更管理やレビュー手法は再説明しない
- `Raspberry Pi` 本で周辺機器活用に触れても、本書は USB 規格そのものの理解を主軸にする
- `OBS` や映像周辺機器と接点があっても、本書では DisplayPort Alt Mode や UVC の境界に留める
- Type-C や PD は深いが、本書では USB を理解するために必要な範囲を優先する
- HID の詳細は本筋ではないが、USB 接続のゲームコントローラーを付録で具体例として扱う

## 本文着手順

1. 1章から5章で USB 2.0 の土台と descriptor / transfer を固める
2. 6章と7章で class と高速側の論点を整理する
3. 8章から10章で Type-C、PD、USB4、Thunderbolt、Alt Mode をつなぐ
4. 11章で観測と試験を厚くする
5. 12章で実装・解析・保守の原則として閉じる
