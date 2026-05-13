# Figure Roughs

## 図1-1 Python を運用コードへ育てる流れ

```mermaid
flowchart LR
  A["外部入力<br/>API / env / config / CLI"] --> B["検証と変換"]
  B --> C["内部モデル"]
  C --> D["CLI / report / batch"]
  D --> E["test / distribution / operation"]
```

## 表4-1 `TypedDict` `dataclass` `Protocol` の使い分け

| 要素 | 向いている場面 | 避けたい場面 |
| --- | --- | --- |
| `TypedDict` | 外部入力の形を表す | 振る舞いを持たせたいとき |
| `dataclass` | 内部モデルを明示したい | 外部スキーマをそのまま持ち込みたいとき |
| `Protocol` | 構造的部分型で契約を表したい | 実体の状態まで固定したいとき |

## 図5-1 例外、ログ、終了コードの関係

```mermaid
flowchart TD
  A["失敗"] --> B["例外として扱う"]
  B --> C["境界でログを書く"]
  C --> D["CLI なら終了コードへ変換"]
```

## 図6-1 テストの層と責務

```mermaid
flowchart TD
  A["単体テスト<br/>変換関数 / 小さな規則"] --> B["統合テスト<br/>ファイル / API / 設定"]
  B --> C["CLI テスト<br/>入口と出力"]
```

## 表7-1 `pyproject.toml` を中心に見た依存関係管理

| 対象 | 主な責務 | 代表例 |
| --- | --- | --- |
| `pyproject.toml` | 依存とメタデータの宣言 | project dependencies |
| 仮想環境 | 実行環境の分離 | `.venv` |
| lock | 再現性の固定 | `uv.lock` |
| build backend | 配布物生成 | hatchling, setuptools |

## 図8-1 同期処理と非同期処理の選択

```mermaid
flowchart TD
  A["仕事の性質を確認"] --> B{"I/O 待ちが支配的か"}
  B -- yes --> C["asyncio を検討"]
  B -- no --> D{"CPU 負荷が高いか"}
  D -- yes --> E["process / native extension を検討"]
  D -- no --> F["同期処理を維持"]
```

## 表9-1 Python 3.14 の実務上の変化

| 項目 | 何が変わるか | 実務での扱い |
| --- | --- | --- |
| annotations | 遅延評価が標準になる | 反射的利用があるなら確認する |
| subinterpreters | 標準ライブラリに入る | まず用途を限定して試す |
| free-threaded | 公式サポートへ進む | 依存ライブラリの追従を見る |
| `compression.zstd` | 標準ライブラリ追加 | 圧縮ワークフローで有力候補 |

## 図10-1 API レスポンスから内部モデルへの流れ

```mermaid
flowchart LR
  A["HTTP response"] --> B["parse raw dict"]
  B --> C["validate required fields"]
  C --> D["convert to dataclass"]
  D --> E["report / CLI / async sync"]
```

## 表11-1 継続運用チェックリスト

| 観点 | 確認項目 |
| --- | --- |
| 設定 | env と config file の責務が分かれているか |
| 境界 | 外部入力を dataclass へ変換しているか |
| テスト | 変換と CLI の両方を押さえているか |
| 更新 | Python と主要依存の更新手順があるか |
