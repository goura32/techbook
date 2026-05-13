# 3. 関数、オブジェクト、ジェネリクスの設計

## この章で伝えること

- 型を実装詳細ではなく API 設計の一部として扱う

## 節構成

### 3-1. 関数シグネチャで契約を表現する

実務の TypeScript では、関数の中身より先に、その関数がどんな契約を持つかが重要になります。引数に何を受け取り、どんな結果を返し、失敗をどう表現するか。この情報が関数シグネチャに乗っていると、読み手は本体を読む前に責務を把握できます。

```ts
type CreateUserInput = {
  email: string;
  displayName: string;
};

type CreateUserResult =
  | { ok: true; userId: string }
  | { ok: false; reason: "duplicate-email" | "invalid-input" };

function createUser(input: CreateUserInput): CreateUserResult {
  // ...
  return { ok: true, userId: "u_123" };
}
```

この例では、成功と失敗の両方が戻り値に現れています。例外を投げる設計が悪いわけではありませんが、「何が起こりうるか」を呼び出し側が事前に把握しやすい点で、こうしたシグネチャは強力です。

逆に、シグネチャが曖昧だと責務も曖昧になります。

```ts
function process(data: any): any {
  // ...
}
```

この関数名と型からは、ほとんど何も分かりません。何を期待し、何を返し、どこで失敗するかが見えないため、変更時の影響範囲も読みにくくなります。

関数シグネチャを設計するときは、少なくとも次の点を意識するとよいです。

- 引数名から文脈が伝わるか
- 戻り値型が成功条件を表しているか
- 失敗が例外なのか戻り値なのか一貫しているか
- オプション引数が責務の曖昧さを生んでいないか

関数本体をきれいにする前に、契約を短く強く書く。この感覚は、後のモジュール設計や公開 API 設計にもそのまま効いてきます。

### 3-2. オブジェクト型と責務の分け方

オブジェクト型は便利ですが、便利だからこそ責務を詰め込みすぎやすい場所でもあります。実務では、ひとつの型に UI 向けの情報、API 向けの情報、更新用の情報、内部フラグまで混ぜ始めると、すぐに読みにくくなります。

```ts
type User = {
  id: string;
  email: string;
  displayName: string;
  isAdmin: boolean;
  canEdit: boolean;
  isSelected: boolean;
  isLoading: boolean;
};
```

この `User` は、一見まとまって見えますが、実際には複数の責務が混ざっています。ドメインとしてのユーザー情報と、画面状態としてのフラグが同居しているからです。こうした型は使い回しやすそうに見えて、文脈が増えるほど壊れやすくなります。

よりよい方向は、「何の文脈で使う型か」を分けることです。

```ts
type User = {
  id: string;
  email: string;
  displayName: string;
  isAdmin: boolean;
};

type UserListItemState = {
  isSelected: boolean;
  isLoading: boolean;
};
```

責務を分けると、型の再利用は一見減るかもしれません。しかし実際には、そのほうが安全に再利用できます。すべてを共通化するより、境界ごとに意味を保ったまま分けたほうが、変更に強いコードになります。

オブジェクト型を設計するときは、「この型は誰のためのものか」を常に問い直すのが有効です。API レスポンスのための型なのか、内部ロジックのためなのか、UI のためなのか。この視点があると、型の大きさや責務の混ざり方に気づきやすくなります。

### 3-3. ジェネリクスはどこまで一般化するべきか

ジェネリクスは TypeScript の魅力のひとつですが、実務で最も誤用されやすい機能のひとつでもあります。問題は、ジェネリクスが難しいことではありません。一般化できるからといって、一般化したほうがよいとは限らないことです。

たとえば、次のような関数はジェネリクスが自然に効きます。

```ts
function first<T>(items: T[]): T | undefined {
  return items[0];
}
```

この関数は、配列の要素型をそのまま返り値へつなぎたいので、ジェネリクスの意味がはっきりしています。`string[]` なら `string | undefined`、`number[]` なら `number | undefined` が返るため、利用者にとっても自然です。

一方で、「将来使うかもしれないから」という理由で何でもジェネリクス化すると、かえって読みにくくなります。

```ts
function createManager<TInput, TOutput, TContext>(
  input: TInput,
  context: TContext,
): TOutput {
  // ...
  throw new Error("not implemented");
}
```

このようなシグネチャは、関数名や文脈が弱いと一気に読みづらくなります。型変数が増えるほど、「その一般化に意味があるか」を厳しく見る必要があります。

ジェネリクスを使う基準としては、次の 3 つが有効です。

- 入力と出力の間に型の対応関係があるか
- 呼び出し側にとって推論が自然か
- 具体型にしたほうが責務が明確にならないか

実務では、「書けるから書く」ではなく、「対応関係を残したいから使う」と考えるのが安全です。特に公開 API では、ジェネリクスの格好よさより、読める契約であることのほうが価値があります。

### 3-4. 条件型と mapped types の使いどころ

条件型や mapped types は、TypeScript を強力にしている機能ですが、同時に可読性を壊しやすい場所でもあります。これらは「複雑なことができる道具」ではなく、「重複する規則を型に落とす道具」と考えるとうまく使いやすくなります。

たとえば、ある型の全プロパティを optional にしたいなら、標準の `Partial<T>` が使えます。読み手にとっても意味がはっきりしています。

```ts
type User = {
  id: string;
  email: string;
  displayName: string;
};

type UserPatch = Partial<User>;
```

一方で、条件型や mapped types を深く入れ子にして独自 DSL のようになってくると、保守性は急激に落ちます。型エラーのメッセージも読みにくくなり、新しく入った開発者が追いづらくなります。

実務での使いどころは、次のような場面に絞るとよいです。

- CRUD 用の派生型を作る
- readonly や optional の一括変換を行う
- 判別可能 union から一部の型を抽出する
- 既存の公開 API の整合性を保つ

逆に、「1 行で何でも表現したい」という動機で型を組み上げると、ほぼ確実に後で苦しくなります。型の抽象化も、関数やクラスの抽象化と同じで、読み手が意図を追える範囲に留めるべきです。

### 3-5. utility types を読む、使う、作りすぎない

TypeScript の標準 utility types は、型の再利用を助ける便利な道具です。`Pick`、`Omit`、`Partial`、`Required`、`Readonly`、`ReturnType` などは、日常的に登場します。まず大切なのは、これらを暗記することより、「何を省略している道具なのか」を理解することです。

```ts
type User = {
  id: string;
  email: string;
  displayName: string;
  passwordHash: string;
};

type PublicUser = Omit<User, "passwordHash">;
```

この例では、公開してよい面だけを `PublicUser` として切り出しています。utility types は、設計意図を短く保ちながら重複を減らすときに力を発揮します。

ただし、utility types に寄りかかりすぎると、「どこから派生した型なのか」は分かっても、「なぜその型が存在するのか」が見えにくくなることがあります。とくに `Pick<Omit<...>>` のような書き方が重なり始めたら、一度立ち止まったほうがよいです。

公開 API や境界では、派生型よりも意味のある名前を持つ独立型のほうが読みやすいことがあります。短く書けることと、分かりやすいことは同じではありません。

独自 utility types も作れますが、それは本当に繰り返しがあり、意図が共有できるときに限るべきです。チーム全体が理解しにくい抽象を増やすより、少し明示的に書いたほうが保守しやすいことは多いです。

この章で見てきた関数、オブジェクト、ジェネリクス、派生型の話は、すべて「型を API 設計の一部として扱う」という一点につながっています。次章では、この設計を最も強く意識すべき場所である、ランタイム境界へ進みます。

## `TaskHub` で API 契約を設計する

`TaskHub` では、同じ「タスクを取得する」処理でも、どの層の契約を書くかで型の置き方が変わります。

```ts
type FetchTasksInput = {
  projectId: string;
  includeArchived?: boolean;
};

type FetchTasksResult =
  | { ok: true; tasks: Task[] }
  | { ok: false; reason: "network-error" | "unauthorized" | "invalid-response" };

function fetchTasks(input: FetchTasksInput): Promise<FetchTasksResult> {
  // ...
  throw new Error("not implemented");
}
```

このシグネチャで大事なのは、HTTP の詳細やレスポンス JSON の生の形ではなく、`api-client` が外へ何を約束するかを先に固定していることです。呼び出し側は `tasks` を受け取るか、`reason` を見て失敗を扱うかに集中できます。

オブジェクト型の責務分割も同じです。`Task` をそのまま UI に流すのではなく、7章では `TaskListItem` のような表示向け型へ変換します。ここで大切なのは、「同じ情報を二重管理しない」ことではなく、「文脈ごとに意味が変わるなら型も分ける」ことです。

ジェネリクスも、`TaskHub` では何でも一般化するためではなく、対応関係を残すために使います。たとえば API レスポンスを包む共通結果型を作るなら、次のような形が自然です。

```ts
type ApiResult<TData, TError extends string> =
  | { ok: true; data: TData }
  | { ok: false; error: TError };
```

この程度の一般化なら、`Task[]` でも `Project[]` でも同じ契約を短く保てます。一方で、型変数が 4 つも 5 つも並び始めたら、その一般化は読者にも将来の自分にも高くつく可能性があります。`TaskHub` のような実務アプリでも、強い抽象化より「各層の契約がすぐ読めること」のほうが価値になる場面は多いです。
