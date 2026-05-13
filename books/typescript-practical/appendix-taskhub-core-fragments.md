# Appendix C. TaskHub の代表断片

## この付録の役割

付録 A は `TaskHub` の構造、付録 B は型と関数の対応を整理するためのものでした。この付録 C では、本文で断片的に登場したコードを、もう少し連続した形で確認できるようにします。

ただし、ここに載せるのはアプリケーション全体の完全実装ではありません。本書の主張に直接関係する「境界」「変換」「公開 API」「実行環境」「画面モデル」が見える最小限の代表断片に絞ります。

## この付録で見るもの

- `shared` の内部モデル
- `api-client` の外部境界と変換
- `cli` の設定読み込みと実行入口
- `web` の画面モデルへの変換

ここで大切なのは、コード量を増やすことではなく、本文で別々に説明した判断がひと続きの流れとして見えることです。

## 想定構成

```text
taskhub/
  apps/
    cli/
      src/
        config.ts
        main.ts
    web/
      src/
        view-models/
          task-list-item.ts
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
```

## 1. `shared` の代表断片

まずは、Node.js 側とフロントエンド側の両方で信頼する内部モデルです。

```ts
// packages/shared/src/task-status.ts
export const taskStatuses = ["open", "done"] as const;

export type TaskStatus = (typeof taskStatuses)[number];

export function parseTaskStatus(value: unknown): TaskStatus {
  if (value === "open" || value === "done") {
    return value;
  }

  throw new Error(`Unknown task status: ${String(value)}`);
}
```

```ts
// packages/shared/src/task.ts
import type { TaskStatus } from "./task-status";

export type Task = {
  id: string;
  title: string;
  status: TaskStatus;
  dueAt: Date | null;
  assigneeName: string | null;
};
```

ここでは、外部 API の事情はまだ登場しません。`shared` はあくまで内部で信頼する型の土台です。

## 2. `api-client` の代表断片

次に、外部 API と接する境界です。ここでは「生レスポンス」と「内部モデル」を分けて持つことが重要でした。

```ts
// packages/api-client/src/types.ts
export type TaskApiResponse = {
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

```ts
// packages/api-client/src/to-task.ts
import type { Task } from "@taskhub/shared/task";
import { parseTaskStatus } from "@taskhub/shared/task-status";
import type { TaskApiResponse } from "./types";

export function toTask(response: TaskApiResponse): Task {
  return {
    id: response.id,
    title: response.title,
    status: parseTaskStatus(response.status),
    dueAt: response.due_at ? new Date(response.due_at) : null,
    assigneeName: response.assignee?.display_name ?? null,
  };
}
```

```ts
// packages/api-client/src/fetch-tasks.ts
import type { Task } from "@taskhub/shared/task";
import type { TaskApiResponse } from "./types";
import { toTask } from "./to-task";

export type FetchTasksInput = {
  apiBaseUrl: string;
  apiToken: string;
  project: string | null;
};

export async function fetchTasks(input: FetchTasksInput): Promise<Task[]> {
  const search = new URLSearchParams();

  if (input.project) {
    search.set("project", input.project);
  }

  const response = await fetch(`${input.apiBaseUrl}/tasks?${search.toString()}`, {
    headers: {
      Authorization: `Bearer ${input.apiToken}`,
    },
  });

  if (!response.ok) {
    throw new Error(`TaskHub request failed: ${response.status}`);
  }

  const json = (await response.json()) as TaskApiResponse[];
  return json.map(toTask);
}
```

この 3 つを並べて見ると、外部仕様の揺れを `api-client` で止めていることがはっきりします。

## 3. `cli` の代表断片

Node.js 側では、環境変数や CLI 引数も外部入力です。そのため、起動入口で設定を確定させる構成を取ります。

```ts
// apps/cli/src/config.ts
export type TaskHubConfig = {
  apiBaseUrl: string;
  apiToken: string;
  defaultProject: string | null;
};

export function loadTaskHubConfig(env: NodeJS.ProcessEnv): TaskHubConfig {
  const apiBaseUrl = env.TASKHUB_API_BASE_URL;
  const apiToken = env.TASKHUB_API_TOKEN;

  if (!apiBaseUrl) {
    throw new Error("TASKHUB_API_BASE_URL is required");
  }

  if (!apiToken) {
    throw new Error("TASKHUB_API_TOKEN is required");
  }

  return {
    apiBaseUrl,
    apiToken,
    defaultProject: env.TASKHUB_DEFAULT_PROJECT ?? null,
  };
}
```

```ts
// apps/cli/src/main.ts
import { fetchTasks } from "@taskhub/api-client/fetch-tasks";
import { loadTaskHubConfig } from "./config";

async function main(): Promise<void> {
  const config = loadTaskHubConfig(process.env);
  const project = process.argv[2] ?? config.defaultProject;

  const tasks = await fetchTasks({
    apiBaseUrl: config.apiBaseUrl,
    apiToken: config.apiToken,
    project,
  });

  for (const task of tasks) {
    const dueLabel = task.dueAt ? task.dueAt.toISOString().slice(0, 10) : "-";
    const assigneeLabel = task.assigneeName ?? "未割り当て";
    console.log(`${task.id}\t${task.status}\t${dueLabel}\t${assigneeLabel}\t${task.title}`);
  }
}

main().catch((error: unknown) => {
  const message = error instanceof Error ? error.message : String(error);
  console.error(message);
  process.exit(1);
});
```

この入口を見ると、CLI が外部 API の生レスポンスも `process.env` も直接引き回していないことが分かります。

## 4. `web` の代表断片

フロントエンド側では、内部モデルをそのまま画面へ流すのではなく、必要に応じて画面モデルへ整えます。

```ts
// apps/web/src/view-models/task-list-item.ts
import type { Task } from "@taskhub/shared/task";

export type TaskListItem = {
  id: string;
  title: string;
  statusLabel: string;
  dueLabel: string;
  assigneeLabel: string;
};

export function toTaskListItem(task: Task): TaskListItem {
  return {
    id: task.id,
    title: task.title,
    statusLabel: task.status === "open" ? "未完了" : "完了",
    dueLabel: task.dueAt ? task.dueAt.toLocaleDateString() : "期限なし",
    assigneeLabel: task.assigneeName ?? "未割り当て",
  };
}
```

この段階では、フレームワーク固有の hook やコンポーネント API へ寄りすぎないようにしています。本書で見せたいのは React の書き方ではなく、表示直前にどこまで意味をそろえるかという設計判断だからです。

## 5. 4 つの断片をどう読むか

この付録で確認してほしいのは、個々のコードそのものより、値の意味が変わる場所です。

1. `TaskApiResponse` は外部都合の型である
2. `toTask()` で内部モデル `Task` へ変換する
3. `loadTaskHubConfig()` で実行環境の値を設定型へ変換する
4. `toTaskListItem()` で画面表示向けの画面モデルへ変換する

つまり、`TaskHub` は 1 つの巨大な共有型で済ませる構成ではありません。境界ごとに型を分け、変換関数を持ち、その結果として利用側の責務を軽くしています。

## 付録 A / B との使い分け

- 構造から見直したいときは付録 A
- 型と関数の役割から見直したいときは付録 B
- ある程度まとまったコード断片で追いたいときは付録 C

本文では行数を抑えるためにコードを細かく分けましたが、付録 C を使うと、それらがどうつながっていたかを一度に見直せます。
