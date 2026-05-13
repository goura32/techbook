# Appendix B. TaskHub のコードマップ

## この付録の役割

この付録は、本文に登場した主要な型と関数を、`TaskHub` 全体の流れの中で見直すためのものです。本文では章ごとに必要な断片だけを示しているため、読後に「`toTask()` はどこで使われていたか」「`Task` と `TaskListItem` の違いは何だったか」をまとめて確認したくなる場面があります。

ここでは、型と関数を単なる一覧として並べるのではなく、外部入力から画面表示までがどうつながるかという順で整理します。

## まず全体の流れを見る

`TaskHub` のコードは、大きく見ると次の流れでつながります。

1. 外部入力を受ける
2. 受け取った値を検証する
3. 内部モデルへ変換する
4. Node.js やフロントエンドで利用する
5. 必要に応じて画面モデルへ再変換する

本文で繰り返してきた `境界 -> 検証 -> 内部モデル -> 利用` という流れを、`TaskHub` の型と関数に落とすと次のようになります。

- 外部 API: `TaskApiResponse`
- 境界での変換: `toTask()`
- 内部で信頼する型: `Task`
- Node.js 側の設定: `TaskHubConfig`
- UI 側の画面モデル: `TaskListItem`
- UI 向け変換: `toTaskListItem()`

## 主要な型

### `TaskStatus`

- 登場章: 2章
- 役割: タスク状態の列挙

`TaskStatus` は、小さな型ですが本書全体の考え方をよく表しています。文字列の自由入力をそのまま許さず、列挙可能な選択肢に固定することで、内部モデルの揺れを抑えます。

```ts
const taskStatuses = ["open", "done"] as const;
type TaskStatus = (typeof taskStatuses)[number];
```

ここで重要なのは、「狭い型を書くこと」自体ではなく、「意味のある選択肢を固定すること」です。

### `Task`

- 登場章: 2章, 4章, 6章, 7章
- 役割: 内部で信頼する基本モデル

`Task` は、`TaskHub` の中心にある内部モデルです。`api-client` が外部レスポンスを受け止めたあと、`cli` と `web` は原則としてこの型を扱います。

```ts
type Task = {
  id: string;
  title: string;
  status: "open" | "done";
  dueAt: Date | null;
  assigneeName: string | null;
};
```

本文で `内部モデル` と呼んでいたものの具体例が、まさにこの `Task` です。

### `TaskApiResponse`

- 登場章: 4章
- 役割: 外部 API の生レスポンス

`TaskApiResponse` は、`Task` と似ていても同じではありません。`snake_case` のフィールド名、文字列の日時、nullable な入れ子など、外部仕様の都合をそのまま持っています。

```ts
type TaskApiResponse = {
  id: string;
  title: string;
  status: "open" | "done";
  due_at: string | null;
  assignee: {
    id: string;
    display_name: string | null;
  } | null;
};
```

この型を `Task` と分けて持つこと自体が、4章の重要な設計判断でした。

### `TaskHubConfig`

- 登場章: 6章
- 役割: CLI / Node.js 側の設定

`TaskHubConfig` は、`process.env` をそのまま使わないための設定型です。Node.js 側では、外部 API と同じくらい環境変数も壊れやすい境界なので、ここを型に落とす意味があります。

```ts
type TaskHubConfig = {
  apiBaseUrl: string;
  apiToken: string;
  defaultProject: string | null;
};
```

### `TaskListItem`

- 登場章: 7章
- 役割: UI 表示向けの画面モデル

`TaskListItem` は、内部モデル `Task` を画面向けに整えた画面モデルです。`Task` が正しいとしても、そのまま UI に流すと表示の都合が混ざりやすくなります。そこで、表示で必要な形に変換した型を別で持ちます。

```ts
type TaskListItem = {
  id: string;
  title: string;
  statusLabel: string;
  dueLabel: string;
  assigneeLabel: string;
};
```

## 主要な関数

### `parseTaskStatus()`

- 登場章: 2章
- 役割: `unknown` から `TaskStatus` を得る

`parseTaskStatus()` は、外部入力をすぐに信じないという原則の最小例です。2章では型システムの土台として登場しましたが、4章以降の境界設計の伏線でもあります。

### `fetchTasks()`

- 登場章: 3章, 6章
- 役割: タスク取得の契約と実装

3章では、`fetchTasks()` は「どんな契約を持つ関数か」を示す例として登場しました。6章では、それが Node.js 実装の中でどう責務を持つかへ進みます。

見たいポイントは 2 つあります。

- 3章では、戻り値が何を約束するか
- 6章では、外部 API を受けて内部モデルを返すまでをどこまで背負うか

同じ関数名でも、章ごとに注目点が違うことを意識すると読みやすくなります。

### `toTask()`

- 登場章: 4章, 8章
- 役割: 外部レスポンスを内部モデルへ変換する

`toTask()` は、`TaskHub` の変換層を代表する関数です。4章では境界設計の中心、8章ではテスト効率の高い関数として扱いました。

```ts
function toTask(response: TaskApiResponse): Task {
  return {
    id: response.id,
    title: response.title,
    status: response.status,
    dueAt: response.due_at ? new Date(response.due_at) : null,
    assigneeName: response.assignee?.display_name ?? null,
  };
}
```

`toTask()` があることで、`cli` と `web` は外部 API の都合から切り離されます。本書の中で最も重要な小関数のひとつです。

### `loadTaskHubConfig()`

- 登場章: 4章, 6章
- 役割: 環境変数から設定を組み立てる

`loadTaskHubConfig()` は、API レスポンスと同じく、環境変数も境界で受け止めるべきだと示す例です。4章では境界一般の一例、6章では Node.js 実装の入り口として機能します。

### `toTaskListItem()`

- 登場章: 7章, 8章
- 役割: 内部モデルを UI 表示向けへ変換する

`toTaskListItem()` は、4章の `toTask()` をフロントエンド側へ持ち込んだような関数です。違いは、外部 API の揺れを吸収するのではなく、内部モデルを画面都合へ整えることにあります。

```ts
function toTaskListItem(task: Task): TaskListItem {
  return {
    id: task.id,
    title: task.title,
    statusLabel: task.status === "open" ? "未完了" : "完了",
    dueLabel: task.dueAt ? task.dueAt.toLocaleDateString() : "期限なし",
    assigneeLabel: task.assigneeName ?? "未割り当て",
  };
}
```

## 章ごとの読み筋

### 2章から4章

この範囲では、`TaskHub` を「型の判断を練習するための小さな題材」として読むのが自然です。

- 2章: `TaskStatus` と `Task`
- 3章: `fetchTasks()` の契約
- 4章: `TaskApiResponse` と `toTask()`

### 5章から7章

この範囲では、`TaskHub` を「どこで境界を引くかを考えるモノレポ例」として読むとつながります。

- 5章: `shared` と `api-client` の公開面
- 6章: `TaskHubConfig` と `fetchTasks()`
- 7章: `Task` から `TaskListItem` への変換

### 8章から9章

この範囲では、`TaskHub` を「どこを支え、どこを検査するかを考える例」として読むと理解しやすくなります。

- 8章: `toTask()` `toTaskListItem()` をどうテストするか
- 9章: `shared` `api-client` `cli` `web` をどう設定で支えるか

## 付録 A との往復ポイント

付録 A では、`TaskHub` を構造から見ました。付録 B では、同じ `TaskHub` をコード断片から見ています。さらに付録 C では、本文で別々に登場した断片をまとまりとして確認できます。迷ったときは、次のように使い分けると便利です。

- 「この型や関数はどこに置くべきか」を確認したいときは付録 A
- 「この型や関数は本文のどこで登場したか」を確認したいときは付録 B
- 「この型や関数が実際にはどう並ぶか」を追いたいときは付録 C

## 最後に見るべきこと

`TaskHub` の付録で本当に持ち帰ってほしいのは、個々の型名や関数名ではありません。次の流れが、1 冊を通して繰り返されていたことです。

- 外部入力をそのまま流さない
- 境界で検証する
- 内部モデルを安定させる
- 利用側の責務に合わせて再度変換する

`TaskHub` は小さな例ですが、この流れは実務の TypeScript でもかなりそのまま使えます。
