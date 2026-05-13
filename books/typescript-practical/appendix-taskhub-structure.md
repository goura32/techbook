# Appendix A. TaskHub の全体構成

## この付録の役割

この付録は、本文中で断片的に登場する `TaskHub` の全体像をまとめて参照するためのものです。本書では各章で必要なコードだけを短く示していますが、それだけだと読者が「そのコードは `TaskHub` のどこにあるのか」「前の章のどの判断とつながっているのか」を見失いやすくなります。

ここでは、`TaskHub` を 1 つの小さなモノレポとして捉え、どの package / app が何を担当するか、どこで境界を引くか、どこまでを公開 API として扱うかを整理します。本文を読み進めるときに迷ったら、まずこの付録へ戻る想定です。

## `TaskHub` の前提

`TaskHub` は、タスク一覧の取得、表示、同期を行う小さなアプリケーション群です。意図的に次の 4 つへ分かれています。

- `packages/shared`
- `packages/api-client`
- `apps/cli`
- `apps/web`

この分割は、実装量を増やすためではありません。TypeScript の実務で難しいのが、単一ファイルの型の書き方ではなく、「複数の境界をまたぐときに何を共有し、何を変換し、どこで責務を切るか」だからです。`TaskHub` では、その判断を小さな例で追えるようにしています。

## 想定ディレクトリ構成

```text
taskhub/
  apps/
    cli/
      src/
        main.ts
        commands/
        output/
    web/
      src/
        pages/
        components/
        view-models/
  packages/
    api-client/
      src/
        fetch-tasks.ts
        to-task.ts
        types.ts
    shared/
      src/
        task-status.ts
        task.ts
  tsconfig.json
  tsconfig.base.json
```

この構成で重要なのは、ディレクトリ名そのものではなく、責務が分かれていることです。`shared` は内部で信頼する型の土台、`api-client` は外部 API 境界、`cli` は Node.js 実行環境、`web` は画面表示と UI 状態を担当します。

## 各 package / app の責務

### `packages/shared`

`shared` は、アプリケーション内部で信頼する型と、ごく小さな共通ユーティリティを置く場所です。本書では主に次のものが属します。

- `TaskStatus`
- `Task`
- 状態の列挙や、内部で再利用する最小限の補助型

ここで意識したいのは、「何でも共有しない」ことです。共有したいのは、Node.js 側でもフロントエンド側でも安定して使える内部モデルです。外部 API のレスポンス型や、画面表示のために整形された文字列群は、ここへ入れません。

### `packages/api-client`

`api-client` は、外部 API と通信する境界です。責務は 3 つあります。

- リクエストを送る
- レスポンスを検証する
- 内部モデルへ変換する

本文では `TaskApiResponse` と `toTask()` がこの層の中心でした。ここがあることで、`cli` や `web` は `snake_case` や日時文字列、外部 API 固有の optional な値を直接扱わずに済みます。

### `apps/cli`

`cli` は、Node.js 実行環境に近い責務を持ちます。たとえば次のようなものです。

- 環境変数からの設定読み込み
- コマンドライン引数の受け取り
- `api-client` 呼び出し
- 標準出力への整形
- ログと終了コード

ここで重要なのは、`cli` が外部 API の生レスポンスを直接知らないことです。`cli` が扱うのは、`api-client` から返ってきた内部モデルです。これによって、実行環境の都合と外部仕様の都合を切り分けやすくなります。

### `apps/web`

`web` は、内部モデルを画面表示へつなぐ層です。責務としては次のものがあります。

- 画面表示
- 画面固有の state 管理
- 画面モデルへの変換
- ユーザー操作の受け取り

本文での `TaskListItem` や `toTaskListItem()` は、この層の判断を示すための例でした。`web` は `Task` をそのまま表示することもありますが、一覧画面や詳細画面では、画面都合の画面モデルへ変換してから使うほうが読みやすくなります。

## 依存方向

`TaskHub` の依存方向は、次のように考えます。

- `shared` は最も内側にある
- `api-client` は `shared` に依存する
- `cli` は `shared` と `api-client` に依存する
- `web` は `shared` と、必要なら `api-client` の公開 API に依存する

逆に、次のような依存は避けます。

- `shared` が `web` 向けの画面モデルを知る
- `web` が `api-client` の内部変換関数へ深く依存する
- `cli` が外部 API の生レスポンス型を直接扱う

この付録で繰り返したいのは、「物理的に同じリポジトリにあること」と「設計上近いこと」は別だという点です。近くにあるから自由に import してよいわけではありません。

## 公開 API の考え方

`TaskHub` の公開 API は、5章の内容を小さく再確認するのにちょうどよい例です。

### `shared` が公開するもの

- `TaskStatus`
- `Task`
- それらに付随する最小限の型

### `shared` が公開しないもの

- 外部 API レスポンスの生型
- UI 専用の画面モデル
- `api-client` や `web` の都合で追加した補助型

### `api-client` が公開するもの

- `fetchTasks()`
- 必要なら `FetchTasksInput` や `FetchTasksResult`

### `api-client` が公開しないもの

- `toTask()`
- レスポンス検証の内部補助関数
- 途中段階の生データ整形関数

この区別を守ると、利用側は「何に依存してよいか」が分かりやすくなります。

## 章との対応

- 1章: `TaskHub` 全体像を導入する
- 2章: `TaskStatus` と `Task` のような内部モデルの土台を見る
- 3章: `fetchTasks()` の契約をどう書くかを見る
- 4章: `TaskApiResponse` と `toTask()` による境界設計を見る
- 5章: package 境界と公開 API を見る
- 6章: `TaskHubConfig` と `fetchTasks()` を Node.js 実行環境へ載せる
- 7章: `Task` から `TaskListItem` への変換を見る
- 8章: どの関数や層をどうテストするかを見る
- 9章: `project references` と設定分割を見る

## この付録の読み方

本文を最初から読むときは、この付録を先に熟読する必要はありません。1章から7章を読む中で、次のようなときに戻ると役に立ちます。

- `Task` と `TaskApiResponse` の違いが曖昧になったとき
- `api-client` と `shared` の責務を思い出したいとき
- `cli` と `web` がどこまで共通化すべきか迷ったとき
- 5章や9章で package 境界と設定境界の関係を確認したいとき

## 付録 B との関係

この付録 A は、`TaskHub` の「構造」を見るためのものです。続く付録 B では、構造の上に置かれた「型と関数の地図」を整理します。さらに付録 C では、代表的なコード断片を少しまとまった形で確認できます。A で場所をつかみ、B で対応関係を追い、必要なら C で断片をまとめて見直す、という順に使うと読みやすくなります。
