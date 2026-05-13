# Appendix A. OpsBox の全体構成

## この付録の役割

この付録は、本文中で断片的に登場する `OpsBox` の全体像をまとめて参照するためのものです。本書では各章で必要なコードだけを短く示しますが、それだけだと読者が「そのコードはどこに置くべきか」「前の章のどの判断とつながっていたか」を見失いやすくなります。

ここでは、`OpsBox` を 1 つの小さな Python パッケージとして捉え、どのモジュールが何を担当するか、どこで境界を引くか、どこまでを公開 API として扱うかを整理します。

## `OpsBox` の前提

`OpsBox` は、外部サービスからジョブやアラートの情報を取得し、内部モデルへ正規化し、CLI で日次レポートを出力するツールです。意図的に次の層へ分けています。

- `opsbox/models.py`
- `opsbox/config.py`
- `opsbox/clients/status_api.py`
- `opsbox/services/reporting.py`
- `opsbox/cli.py`
- `opsbox/async_sync.py`

この分割は、実装量を増やすためではありません。Python の実務で難しいのは、単一ファイルの書き方よりも、「設定」「外部 API」「内部モデル」「CLI」を混ぜずに責務を分けることだからです。

## 想定ディレクトリ構成

```text
opsbox/
  pyproject.toml
  src/
    opsbox/
      __init__.py
      cli.py
      config.py
      models.py
      async_sync.py
      clients/
        status_api.py
      services/
        reporting.py
  tests/
    test_config.py
    test_reporting.py
    test_cli.py
```

## 各モジュールの責務

### `models.py`

- 内部で信頼する dataclass
- 状態の列挙
- 外部仕様を持ち込まない中心モデル

### `config.py`

- 環境変数と設定ファイルの読み込み
- デフォルト値の決定
- CLI から見た実行環境の入り口

### `clients/status_api.py`

- 外部 API との通信
- 生レスポンスの解析
- 内部モデルへの変換

### `services/reporting.py`

- 内部モデルからレポート向けの値を組み立てる
- CLI やバッチ処理が直接持つべきでない集約ロジックを置く

### `cli.py`

- 引数解析
- 設定読み込み
- サービス呼び出し
- 出力と終了コード

### `async_sync.py`

- 並列取得や待ち合わせ
- `asyncio` 導入が意味を持つ I/O を限定して扱う

## 依存方向

- `models.py` は最も内側にある
- `config.py` と `clients/status_api.py` は `models.py` に依存する
- `services/reporting.py` は `models.py` に依存する
- `cli.py` は `config.py` `clients/status_api.py` `services/reporting.py` に依存する
- `async_sync.py` は `clients/status_api.py` と `models.py` に依存する

逆に、次のような依存は避けます。

- `models.py` が外部 API の生辞書を知る
- `services/reporting.py` が環境変数を直接読む
- `cli.py` が外部レスポンス辞書をそのまま整形する

## 公開 API の考え方

### 外へ見せるもの

- `load_settings()`
- `fetch_jobs()`
- `build_daily_report()`
- `main()`

### 外へ見せないもの

- 生レスポンスの補助関数
- 設定ファイル内部の細かなパース関数
- CLI 専用の表示整形補助

## 章との対応

- 1章: `OpsBox` 全体像を導入する
- 2章: dataclass と責務分割を見る
- 3章: 標準ライブラリの担当範囲を見る
- 4章: `TypedDict` と dataclass の境界を見る
- 5章: 設定、例外、ログ、CLI を見る
- 6章: `pytest` でどこを支えるかを見る
- 7章: `pyproject.toml` と配布を載せる
- 8章: `async_sync.py` で非同期導入の範囲を見る
- 10章: 外部 API レスポンスを内部モデルへ変換する

## 付録 B / C との関係

付録 A は `OpsBox` の構造を見るためのものです。付録 B ではその上にある型と関数の対応を、付録 C ではまとまったコード断片を確認できます。A で場所をつかみ、B で役割を追い、必要なら C でまとめて見直す使い方が自然です。
