# 4. ランタイム境界とバリデーション

## この章で伝えること

- 外部入力は型だけでは守れないことを明確にし、検証の置き場所を示す

## 節構成

### 4-1. 型だけでは守れない入力

ここまでの章では、TypeScript の型システムを使って、コードの内側にある契約をどう明確にするかを見てきました。しかし、実務のコードが壊れる場所は、たいていコードの外側との接点にあります。HTTP レスポンス、フォーム入力、環境変数、ファイル、CLI 引数、データベース、キュー、Webhook。これらはすべて、TypeScript の静的な世界の外からやってきます。

この章の一般原則は、本書全体で共有している「境界で入力を扱う基本原則」ともつながっています。本書では、その一般論を `TaskHub` の API クライアントと設定読み込みにどう適用するかへ絞って見ていきます。

重要なのは、TypeScript は「ソースコードに書いた前提」を検査できても、「実際に流れてくる値」がその前提を満たしているかまでは保証できないことです。

```ts
type User = {
  id: string;
  displayName: string;
};

async function fetchUser(userId: string): Promise<User> {
  const response = await fetch(`/api/users/${userId}`);
  return response.json();
}
```

このコードは一見自然に見えますが、実際には危うい書き方です。`response.json()` が返す値は、TypeScript にとっては本来 `unknown` に近いものです。`User` と書いた瞬間に安全になったわけではなく、「そうであってほしい」と宣言しただけです。

こうした書き方が危険なのは、壊れる場所が遠くなるからです。問題のあるレスポンスを受け取った瞬間ではなく、その何十行も先で `displayName.toUpperCase()` のような形で落ちると、原因が追いにくくなります。

ランタイム境界で必要なのは、型注釈ではなく確認です。つまり、

- 何が外部入力なのかを明確にする
- その入力をいつ検証するかを決める
- 検証後の内部表現を別の型として扱う

という流れです。TypeScript の型は、その流れを整理するためには非常に役立ちますが、検証そのものの代わりにはなりません。

### 4-2. API、JSON、フォーム、環境変数の境界

境界とひとことで言っても、実務ではいくつかの種類があります。それぞれ壊れ方が違うため、検証のしかたも少しずつ変わります。

まず典型的なのは API レスポンスです。外部サービスやバックエンド API は、ドキュメント通りに見えても、実際には `null` が混ざったり、フィールド名が変わったり、デプロイのタイミングで部分的に不整合が出たりします。特に自分たちが管理していない API では、「いつでも壊れうる入力」として扱ったほうが安全です。

フォーム入力も同じです。見た目では数値入力欄でも、ブラウザが返す値は基本的に文字列です。空文字も来ますし、ユーザーが想定外の操作をすることもあります。`<input type="number">` があるから大丈夫、とは考えないほうがよいです。

環境変数もよく誤解される境界です。`process.env.PORT` や `process.env.NODE_ENV` は、名前からすると安定した設定値に見えますが、実際には単なる文字列か `undefined` です。しかも、設定ミスは本番で初めて発覚することがあります。

CLI 引数、設定ファイル、JSON ファイル、ローカルストレージなども、本質的には同じ種類の問題を持っています。つまり、「見た目には構造化されているが、受け取る側から見ると信用できない入力」です。

この章で繰り返し使う考え方は、境界ごとに次の 3 段階へ分けることです。

1. 生の入力を受け取る
2. 検証と変換を行う
3. 内部で使う型へ落とす

たとえば環境変数なら、次のようなイメージになります。

```ts
type AppConfig = {
  port: number;
  logLevel: "debug" | "info" | "warn" | "error";
};

function loadConfig(env: NodeJS.ProcessEnv): AppConfig {
  const rawPort = env.PORT;
  const rawLogLevel = env.LOG_LEVEL;

  const port = rawPort ? Number(rawPort) : 3000;

  if (!Number.isInteger(port)) {
    throw new Error("PORT must be an integer");
  }

  if (
    rawLogLevel !== "debug" &&
    rawLogLevel !== "info" &&
    rawLogLevel !== "warn" &&
    rawLogLevel !== "error"
  ) {
    throw new Error("LOG_LEVEL is invalid");
  }

  return {
    port,
    logLevel: rawLogLevel,
  };
}
```

この `loadConfig` の役割は、`env` をそのまま使わないことです。アプリケーションの残りの部分では `process.env` を直接触らず、検証済みの `AppConfig` だけを使うようにすると、設定まわりの事故はかなり減ります。

`TaskHub` では、まず `api-client` で受け取るレスポンスと `cli` の設定読み込みを題材に、この境界整理を行います。以降の章では、ここで定義した「外部入力 -> 検証 -> 内部モデル」という流れを前提に進めます。

たとえば `TaskHub` の外部 API が、次のようなレスポンスを返すとします。

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

この形をそのままアプリケーション全体へ流すと、`due_at` が文字列であること、`display_name` が `null` かもしれないこと、`assignee` 自体が存在しないことまで、あらゆる利用側が知る必要があります。本書では、こうした生のレスポンスを境界で受け止め、内部モデルへ変換する流れを繰り返し使います。

### 4-3. パースと検証をどこに置くか

境界で大事なのは、「検証が必要だ」と分かることだけではありません。どこでそれを行うかを決めることも同じくらい重要です。検証の置き場所が曖昧だと、同じ入力を複数箇所でチェックしたり、逆に誰もチェックしなかったりします。

基本方針としては、境界に最も近い場所で一度だけ検証するのがよいです。受け取るたびにあちこちで `typeof` を書くのではなく、入口で「生の値」を「内部で使える値」に変換します。

たとえば API クライアントなら、次の 2 つを分けるのが有効です。

- データを取得する層
- 取得したデータをアプリケーション用の型へ変換する層

```ts
type ApiUser = {
  id: string;
  display_name: string | null;
};

type User = {
  id: string;
  displayName: string;
};

function toUser(apiUser: ApiUser): User {
  return {
    id: apiUser.id,
    displayName: apiUser.display_name ?? "Unknown",
  };
}
```

ここでは `ApiUser` 自体が「すでに検証済みの外部型」である前提ですが、さらに手前で JSON の形を検証しておけば、`toUser()` は変換だけに集中できます。

`TaskHub` でも考え方は同じです。

```ts
type Task = {
  id: string;
  title: string;
  status: "open" | "done";
  dueAt: Date | null;
  assigneeName: string | null;
};

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

この `toTask()` があることで、以降の CLI や UI は `snake_case` の外部仕様や日時文字列の扱いを知らずに済みます。4章の主題は、この「変換を境界で閉じ込める」ことです。`TaskHub` 全体の流れに戻って確認したい場合は、付録 B にある `TaskApiResponse -> toTask() -> Task` の整理を見返すとつながりやすくなります。

一方で、検証を深い場所へ押し込むと問題が起きます。たとえば UI コンポーネントの中で毎回 `user && typeof user.id === "string"` のようなチェックを始めると、境界がどこなのかが分からなくなります。UI は表示ロジックに集中したいので、そこで入力の正しさまで背負わせるべきではありません。

パースと検証を置く場所の判断基準は、次のように整理できます。

- 入口でまとめて失敗させたいか
- 部分的に不正でも処理を続けたいか
- 呼び出し側へどこまで失敗理由を返したいか
- 変換後の内部型をどこまで安定させたいか

つまり、検証は文法的な話ではなく、アーキテクチャの話です。どこで責任を持つかが決まると、型の置き方も自然に決まってきます。

### 4-4. 失敗を型に乗せる方法

境界で検証を行うと、当然ながら失敗が発生します。ここで設計上の分岐になります。失敗を例外として扱うのか、戻り値として表現するのか。それぞれに利点があります。

設定の読み込みやアプリケーション起動のように、「失敗したらその場で止めるべき」処理では、例外を投げる設計は自然です。逆に、フォーム検証や API 入力のバリデーションのように、「失敗を呼び出し側で扱う」必要がある場面では、戻り値として失敗を表現したほうが扱いやすいです。

```ts
type ParseResult<T> =
  | { ok: true; value: T }
  | { ok: false; issues: string[] };

function parsePort(value: unknown): ParseResult<number> {
  if (typeof value !== "string") {
    return { ok: false, issues: ["port must be a string"] };
  }

  const port = Number(value);

  if (!Number.isInteger(port)) {
    return { ok: false, issues: ["port must be an integer"] };
  }

  return { ok: true, value: port };
}
```

この設計の利点は、呼び出し側が失敗を無視しにくいことです。`ok` を見て分岐することで、成功時の値だけを安全に扱えます。さらに、失敗の理由を UI やログへ流しやすくなります。

```ts
const result = parsePort(process.env.PORT);

if (!result.ok) {
  console.error(result.issues.join(", "));
  process.exit(1);
}

const port = result.value;
```

ただし、すべてを戻り値にする必要はありません。境界の失敗をどこで吸収するかに応じて、例外と `Result` 型を使い分けるのが実務的です。重要なのは、失敗の扱いがその場しのぎで揺れないことです。同じ種類の入力に対して、ある箇所では例外、別の箇所では `null`、さらに別の箇所では空文字列、という状態になると、読者にも呼び出し側にも負担がかかります。

### 4-5. バリデーションライブラリ選定の観点

実務で TypeScript を使っていると、どこかでバリデーションライブラリを導入するかどうかを考えることになります。手書きの `typeof` チェックだけで十分な場面もありますが、API スキーマやフォーム入力、設定ファイル、複雑な union を扱い始めると、専用ライブラリの恩恵は大きくなります。

ただし、本書の立場ははっきりしています。先に覚えるべきなのはライブラリ名ではなく、境界で何を検証し、何を内部型へ変換するかという原則です。ライブラリはその原則を実装しやすくする道具であって、原則そのものの代わりではありません。そのため本書では、4章の前半を手書きの検証で説明し、ここでは「実務ではこう置き換えられる」という代表例だけを短く扱います。

ここで重要なのは、「どのライブラリが最強か」ではなく、「自分たちの境界に何が必要か」を見ることです。選定時には、少なくとも次の観点を持っておくと判断しやすくなります。

- ランタイム検証が主目的か
- 型推論の精度がどれくらい必要か
- エラーメッセージをどの程度整形したいか
- フォームや API スキーマと統合したいか
- チームが読める記法か

たとえば、`zod` のように TypeScript との親和性が高く、スキーマから型を推論できるライブラリは、フロントエンドとバックエンドの境界で扱いやすいことがあります。一方で、JSON Schema や OpenAPI ベースの世界と深く連携したいなら、別の選択が自然な場合もあります。

`TaskHub` の `TaskApiResponse` を、ごく短く `zod` 風に書き直すと、次のようなイメージになります。

```ts
const TaskApiResponseSchema = z.object({
  id: z.string(),
  title: z.string(),
  status: z.union([z.literal("open"), z.literal("done")]),
  due_at: z.string().nullable(),
  assignee: z
    .object({
      id: z.string(),
      display_name: z.string().nullable(),
    })
    .nullable(),
});

type TaskApiResponse = z.infer<typeof TaskApiResponseSchema>;
```

この形の利点は、ランタイム検証と型定義を近い場所へ寄せやすいことです。ただし、ここで本当に大切なのは `z.object(...)` の書き方ではありません。次の 2 点です。

- 境界で生の入力を検証すること
- 検証後も `toTask()` のような変換層を残すこと

つまり、`zod` を使っても `TaskApiResponse` と `Task` を同一視しない、という 4章の原則は変わりません。ライブラリを導入しても、外部仕様を内部モデルへどう落とすかという設計判断は残ります。

ただし、ライブラリ導入で気をつけたいのは、「スキーマを書いたから設計が良くなるわけではない」ことです。ライブラリは検証を助けますが、どの境界で何を内部型へ変換するか、失敗をどう扱うか、どこまで外部仕様を内部へ持ち込むかといった設計判断は残ります。

本書では、具体的な実例としてライブラリに触れる場面はありますが、依存しすぎない立場を取ります。6章や7章ではライブラリ名を前面に出すより、「検証ライブラリを使うにせよ使わないにせよ、境界で入力を放置しない」という原則を優先します。大切なのは、ツールの名前よりも、その原則を崩さないことです。
