# Terminology

このファイルでは、全書籍で統一したい用語と表記を管理します。

## 基本ルール

- 初出では必要に応じて英語原語を併記する
- 2回目以降は、読者にとって自然な統一表記を使う
- コード上の正式名称と本文上の説明語を混同しない

## 用語集

| 用語 | 統一表記 | 補足 |
| --- | --- | --- |
| repository | リポジトリ | `repo` は口語的説明に限定 |
| monorepo | モノレポ | 必要なら初出で monorepo を併記 |
| CLI | CLI | コマンドラインインターフェースと初出で補足可 |
| API | API | Application Programming Interface の補足は初出のみ |
| SDK | SDK | Software Development Kit の補足は初出のみ |
| public API | 公開 API | 本文ではこの表記に統一 |
| internal model | 内部モデル | 外部入力を変換した後の、内部で信頼する型 |
| transformation layer | 変換層 | 境界で値を内部モデルや画面モデルへ変換する層 |
| view model | 画面モデル | 初出では view model を併記してよい |
| assertion | アサーション | 型アサーションの文脈ではこの表記に統一 |
| branch | ブランチ | Git 文脈ではこの表記に統一 |
| pull request | Pull Request | 略して `PR` を使う場合は初出で併記 |
| environment variable | 環境変数 | `ENV` と混在させない |
| setup | セットアップ | 動詞は「セットアップする」 |
| build | ビルド | 動詞は「ビルドする」 |
| deploy | デプロイ | 動詞は「デプロイする」 |

## 書籍追加時の運用

- 新しい用語ルールを追加したら、この一覧を更新する
- 書籍固有の訳語を採用する場合は、`books/<book-id>/writing-policy.md` に理由を書く
