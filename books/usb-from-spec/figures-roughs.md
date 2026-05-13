# Figure Roughs

## 図1-1 USB を層で見る見取り図

```mermaid
flowchart TD
  A["Connector / Cable"] --> B["Power / Role"]
  B --> C["Enumeration / Descriptors"]
  C --> D["Transfers / Classes"]
  D --> E["Driver / Application"]
```

## 図2-1 host、hub、device、endpoint の関係

```mermaid
flowchart LR
  H["Host"] --> HB["Hub"]
  HB --> D1["Device A"]
  HB --> D2["Device B"]
  D1 --> E1["Endpoint 1"]
  D1 --> E2["Endpoint 2"]
```

## 図3-1 attach から enumeration までの流れ

```mermaid
flowchart TD
  A["Attach"] --> B["Reset"]
  B --> C["Read Device Descriptor"]
  C --> D["Set Address"]
  D --> E["Read Configuration"]
  E --> F["Set Configuration"]
  F --> G["Class-specific Init"]
```

## 表3-1 主な descriptor の役割

| Descriptor | 何を表すか | よく迷う点 |
| --- | --- | --- |
| Device | デバイス全体 | class を device に置くか interface に置くか |
| Configuration | 構成全体 | 電力値や interface 数 |
| Interface | 機能単位 | class / subclass / protocol |
| Endpoint | 転送口 | transfer type と最大パケットサイズ |

## 図4-1 4 種類の転送方式の比較

```mermaid
flowchart LR
  C["Control"] --> P["設定と列挙"]
  B["Bulk"] --> Q["大きなデータ"]
  I["Interrupt"] --> R["短い定期通知"]
  S["Isochronous"] --> T["時間優先ストリーム"]
```

## 表4-1 control / bulk / interrupt / isochronous の使い分け

| 転送 | 向いている用途 | 強み | 弱み |
| --- | --- | --- | --- |
| Control | 設定、列挙 | 必須、汎用 | 帯域は小さい |
| Bulk | ログ、ストレージ | 完全性 | 遅延保証は弱い |
| Interrupt | 入力イベント | 周期性 | 大量データには不向き |
| Isochronous | 音声、映像 | 時間優先 | 再送しない |

## 図5-1 legacy connector と Type-C の切り分け

```mermaid
flowchart TD
  A["Connector Shape"] --> B["Legacy A/B/Micro"]
  A --> C["USB Type-C"]
  C --> D["Role Signaling"]
  C --> E["Cable Capability"]
```

## 図6-1 USB Type-C と USB PD の電力交渉

```mermaid
flowchart LR
  S["Source"] --> N["Negotiation"]
  K["Sink"] --> N
  N --> V["Voltage / Current Contract"]
```

## 表6-1 power role と data role の組み合わせ

| power role | data role | ありうる例 |
| --- | --- | --- |
| Source | Host | PC から周辺機器へ給電 |
| Sink | Device | バスパワー機器 |
| Source | Device | 外部電源付きアクセサリ |
| Sink | Host | 電力を受けるノート PC |

## 図7-1 USB 2.0、USB 3.2、USB4 の関係

```mermaid
flowchart LR
  U2["USB 2.0"] --> U32["USB 3.2"]
  U32 --> U4["USB4"]
  TC["USB Type-C"] --> U32
  TC --> U4
```

## 表8-1 代表的な device class と host 側の見え方

| Class | 典型用途 | host 側での見え方 |
| --- | --- | --- |
| HID | 入力、簡易制御 | driver を用意しやすい |
| CDC | シリアル通信 | 仮想 COM / tty |
| MSC | ストレージ | block device |
| Vendor-specific | 独自機能 | 自前 driver / library |

## 図9-1 `TraceDock` の構成と descriptor の配置

```mermaid
flowchart TD
  A["Type-C Receptacle"] --> B["PD / Power"]
  A --> C["USB Device Controller"]
  C --> D["HID Interface"]
  C --> E["Bulk Logging Interface"]
  F["Host Tool"] --> D
  F --> E
```

## 図10-1 debug の切り分け順

```mermaid
flowchart TD
  A["Power / Cable"] --> B["Enumeration"]
  B --> C["Descriptor"]
  C --> D["Class / Driver"]
  D --> E["Application"]
```

## 表10-1 試験仕様と analyzer の役割差

| 手段 | 何を見るか | 向いている場面 |
| --- | --- | --- |
| Interop Test | host/device の組み合わせ | 相互接続の再現 |
| Electrical Test | 信号品質 | 物理層問題 |
| Protocol Analyzer | packet / request | 列挙と転送の解析 |
| Descriptor Dump | 構成情報 | class / interface の確認 |
