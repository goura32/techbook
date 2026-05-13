# Outline

## 書籍の目的

- USB を単なるコネクタや転送速度の話としてではなく、ホスト、デバイス、ケーブル、電力、列挙、転送、デバッグまで一貫したシステムとして理解できるようにする
- USB 2.0、USB 3.2、USB Type-C、USB Power Delivery の関係を、実装と解析の判断軸が残る形で整理する

## 仮タイトル

- 技術の輪郭 USB
- サブタイトル案: 列挙、転送、Type-C、PD、解析までを実装視点でつなぐ

## 通しサンプル

- 題材名: `TraceDock`
- 概要:
  - USB Type-C 接続の小型測定デバイス
  - 通常動作では HID で制御し、bulk 転送でログを取得する
  - 高負荷時は USB PD による追加電力を使い、ホスト側ツールで状態確認とファームウェア更新を行う
- ねらい:
  - 列挙、descriptor、転送方式、Type-C、PD、解析手順がどこでつながるかを同じ題材で追えるようにする
  - 「USB がつながらない」ときに、コネクタ、電力、列挙、クラス、ドライバのどこを疑うべきかを見えるようにする
  - 規格書の情報を、実装判断とデバッグ判断へ変換する流れを章ごとに示す

## 章構成案

1. USB を仕様から理解するための見取り図
2. バス、トポロジ、ホストとデバイスの役割
3. 列挙、descriptor、USB 2.0 仕様 Chapter 9 の基本
4. 転送方式とスケジューリング
5. コネクタ、ケーブル、USB Type-C
6. 電力供給、USB PD、役割交渉
7. USB 3.2、USB4、その前後関係
8. デバイスクラス、ドライバ、OS の見え方
9. `TraceDock` を実装視点で読む
10. 解析、テスト、コンプライアンス
11. 長く保守できる USB 製品設計

## 詳細構成

### 1. USB を仕様から理解するための見取り図

- 1-1. USB をコネクタ名で覚えない
- 1-2. データ、電力、役割、世代の話を分ける
- 1-3. `境界で切り分け、観測点を持つ`
- 1-4. 本書の対象範囲
- 1-5. 通しサンプル `TraceDock` の紹介

### 2. バス、トポロジ、ホストとデバイスの役割

- 2-1. host、hub、device をどう見るか
- 2-2. endpoint と interface の考え方
- 2-3. USB 2.0 時代の基本トポロジ
- 2-4. role の混同を避ける
- 2-5. USB が壊れる場所はどこか

### 3. 列挙、descriptor、USB 2.0 仕様 Chapter 9 の基本

- 3-1. attach から enumeration までの流れ
- 3-2. device descriptor、configuration descriptor、string descriptor
- 3-3. interface と endpoint の見え方
- 3-4. request と status stage
- 3-5. descriptor を読めるようになる価値

### 4. 転送方式とスケジューリング

- 4-1. control、bulk、interrupt、isochronous の違い
- 4-2. 帯域、遅延、再送の考え方
- 4-3. polling と host 主導
- 4-4. 何をどの転送へ載せるか
- 4-5. 転送方式の選択がデバッグへ与える影響

### 5. コネクタ、ケーブル、USB Type-C

- 5-1. Standard-A/B、Micro-USB、Type-C の役割差
- 5-2. Type-C は形状だけの話ではない
- 5-3. cable quality と signal path
- 5-4. alternate mode と USB の切り分け
- 5-5. 物理層の問題を論理層のせいにしない

### 6. 電力供給、USB PD、役割交渉

- 6-1. USB の電力をデータ転送の延長で見ない
- 6-2. source、sink、power role
- 6-3. USB PD 3.2 と EPR
- 6-4. Type-C と PD の関係
- 6-5. 電力交渉の失敗をどう見るか

### 7. USB 3.2、USB4、その前後関係

- 7-1. USB 2.0 と USB 3.x を混同しない
- 7-2. USB 3.2 の naming と lane
- 7-3. backwards compatibility の実態
- 7-4. USB4 をどこまで知ればよいか
- 7-5. 世代差分を marketing 文言で理解しない

### 8. デバイスクラス、ドライバ、OS の見え方

- 8-1. class、subclass、protocol の役割
- 8-2. HID、MSC、CDC の代表例
- 8-3. class 準拠と vendor-specific の分岐
- 8-4. OS ごとの差がどこで出るか
- 8-5. ドライバ問題と descriptor 問題を切り分ける

### 9. `TraceDock` を実装視点で読む

- 9-1. どの descriptor を持たせるか
- 9-2. HID 制御と bulk ログ転送の分離
- 9-3. 電力設計と Type-C 前提の制約
- 9-4. host 側ツールの責務
- 9-5. 仕様を最小限に絞る

### 10. 解析、テスト、コンプライアンス

- 10-1. どこまで analyzer が必要か
- 10-2. xHCI interop と electrical test の違い
- 10-3. ログ、descriptor dump、power trace の見方
- 10-4. 再現手順と切り分け
- 10-5. 認証と実装品質を混同しない

### 11. 長く保守できる USB 製品設計

- 11-1. 規格の全部を一度に実装しない
- 11-2. host 側と device 側の責務を固定する
- 11-3. ケーブル依存、OS 依存、電力依存を見える化する
- 11-4. 仕様更新への追従方法
- 11-5. USB を選ばない判断も持つ

## 各章の要点

- 1章: USB を「形状」ではなく「層の重なり」として捉え直す
- 2章: host、device、endpoint、role の基本を整理する
- 3章: 列挙と descriptor を読む力を土台にする
- 4章: 4 種類の転送方式を、性能とデバッグの観点で整理する
- 5章: Type-C とケーブル問題を物理層の現実として扱う
- 6章: USB PD と power role を、データ転送とは別の軸として理解する
- 7章: USB 3.2 と USB4 の位置づけを marketing 名称から切り離して整理する
- 8章: class と driver の関係を OS 観点で見る
- 9章: `TraceDock` を使って実装時の選択を具体化する
- 10章: 解析、テスト、コンプライアンスを切り分ける
- 11章: 長期保守と仕様追従の考え方で閉じる

## 他書との役割分担メモ

- `Git & GitHub` の変更管理やレビュー手法は再説明しない
- `Hermes Agent` のような OSS ツール側の USB デバイス利用には触れても、AI エージェントの説明へは逸れない
- `Raspberry Pi` や `OBS` と将来役割が重なっても、本書は USB そのものの理解と解析を主軸にする
- Type-C と PD は重要だが、それ自体の完全な分冊本へは踏み込まず、USB を理解するための範囲へ絞る

## 本文着手順

1. 1章で USB を読むための視点と `TraceDock` を定義する
2. 2章から4章で bus、enumeration、transfer の土台を固める
3. 5章から7章で connector、Type-C、PD、世代差分を整理する
4. 8章から10章で class、実装、解析、コンプライアンスをつなぐ
5. 11章で保守と仕様追従の原則として閉じる
