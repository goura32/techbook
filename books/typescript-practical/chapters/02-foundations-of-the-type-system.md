# 2. 型システムの土台

## この章で伝えること

- TypeScript の基本的な推論と型の厳しさを、実務で使う判断軸に変える

## 節構成

### 2-1. 推論と明示注釈の役割分担

TypeScript の大きな利点のひとつは、毎回すべての型を書かなくても、多くの情報を推論してくれることです。推論があるおかげで、コードは過度に冗長にならず、読み手も値の意味に集中しやすくなります。

```ts
const prices = [100, 200, 300];
const total = prices.reduce((sum, price) => sum + price, 0);
```

この例では、`prices` は `number[]`、`total` は `number` と自然に推論されます。この程度の場面で毎回注釈を書く必要はありません。むしろ、明らかなところに注釈を増やしすぎると、本当に見たい契約が埋もれます。

一方で、境界や公開面では明示注釈が重要になります。特に次の場所では、推論に頼りきらないほうが安全です。

- 関数の戻り値
- 公開されるオブジェクトの型
- ライブラリやモジュールのエクスポート
- 境界で受け取るデータの変換結果

```ts
export function parsePort(value: string | undefined): number {
  if (value == null) return 3000;

  const port = Number(value);
  return Number.isInteger(port) ? port : 3000;
}
```

この関数で戻り値型を書いておくと、後から `number | undefined` を返してしまったときにすぐ気づけます。公開された関数では、推論の結果より「契約を固定する」ことのほうが重要です。

実務では、ローカル変数では推論を活かし、境界では注釈を強めるのが基本方針になります。つまり、「全部書く」でも「全部任せる」でもなく、どこに契約を置くかで使い分けることが大切です。

### 2-2. `unknown` `any` `never` をどう使い分けるか

TypeScript を使い始めたばかりのとき、多くの人が最初に困るのが `any` です。`any` を付ければエラーは消えますが、それは型システムとの接点を切ってしまうことでもあります。`any` は便利ですが、使った場所から先は TypeScript の支援が急に弱くなります。

```ts
function getName(user: any) {
  return user.profile.name;
}
```

このコードは一見書きやすいですが、`user.profile` がない場合や `name` が文字列でない場合でも、静的には止まりません。`any` は「まだ型が分からない」ではなく、「ここでは何も検査しない」という意味に近いものです。

ここで重要になるのが `unknown` です。`unknown` は「何が入っているか分からない」ことを正直に表現しますが、そのままでは何もできません。使う前に確認が必要です。

```ts
function getName(value: unknown): string | undefined {
  if (
    typeof value === "object" &&
    value !== null &&
    "profile" in value
  ) {
    const profile = value.profile;

    if (
      typeof profile === "object" &&
      profile !== null &&
      "name" in profile &&
      typeof profile.name === "string"
    ) {
      return profile.name;
    }
  }

  return undefined;
}
```

この例は冗長ですが、意味ははっきりしています。`unknown` は曖昧さを隠さず、確認の責務を呼び出し側か変換層に押し戻します。外部入力を受ける箇所では、`unknown` のほうが `any` よりもはるかに安全です。

`never` は少し性格が違います。`never` は「値が存在しない」「ここへ到達しない」ことを表します。実務では、網羅性確認に役立ちます。

```ts
type Status = "draft" | "published" | "archived";

function renderStatus(status: Status): string {
  switch (status) {
    case "draft":
      return "Draft";
    case "published":
      return "Published";
    case "archived":
      return "Archived";
    default: {
      const unreachable: never = status;
      return unreachable;
    }
  }
}
```

あとから `Status` に新しい値が追加されたとき、この `never` があると switch の取りこぼしに気づけます。`never` は難解な型テクニックというより、変更漏れを防ぐための警報装置として理解すると使いやすくなります。

まとめると、`any` は最後の逃げ道、`unknown` は境界での正直な出発点、`never` は網羅性と到達不能性の確認に使う、という整理が実務では有効です。

### 2-3. narrowing と control flow analysis

TypeScript の便利さが本当に効いてくるのは、値を「いま何であるか」に応じて絞り込めるときです。これが narrowing です。TypeScript は `typeof`、`instanceof`、`in`、比較演算、`null` チェックなどから、コードの流れに沿って型を絞り込みます。

```ts
function print(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
    return;
  }

  console.log(value.toFixed(2));
}
```

この関数では、`if` の内側で `value` は `string`、その外では `number` として扱われます。こうした絞り込みは、型注釈を増やすことよりも「処理の分岐を読みやすくする」効果が大きいです。

もう少し実務的には、判別可能な union が重要です。

```ts
type Result<T> =
  | { ok: true; value: T }
  | { ok: false; error: string };

function handleResult(result: Result<number>) {
  if (result.ok) {
    return result.value * 2;
  }

  throw new Error(result.error);
}
```

`ok` という共通の識別子があることで、処理の分岐と型の分岐が一致します。実務では、曖昧な `null` や例外だけに頼るより、こうした union を使って成功と失敗を表現したほうが、変更に強いコードになりやすいです。

control flow analysis が効きやすいコードは、分岐条件がはっきりしています。逆に、複数の意味を持つフラグや、途中で値の意味が変わる変数は、型システムにも人間にも読みにくくなります。型のためにコードを書く必要はありませんが、型が素直に読めるコードは、たいてい人間にも読みやすいです。

### 2-4. nullability と optional の設計

実務の TypeScript で事故が多いのは、高度な型テクニックよりも `null`、`undefined`、optional property の扱いです。値が「ない」ことをどう表現するかが曖昧だと、呼び出し側の分岐が増え、コードベース全体の前提も崩れやすくなります。

```ts
type User = {
  id: string;
  nickname?: string;
};
```

この `nickname?: string` は便利ですが、「プロパティ自体が存在しない」と「文字列がまだ設定されていない」を同じ意味で使い始めると、設計がぶれます。場合によっては、次のように `null` を明示したほうが意味がはっきりします。

```ts
type User = {
  id: string;
  nickname: string | null;
};
```

どちらが正しいかは文脈次第です。重要なのは、optional を「雑に省略できる便利な書き方」として使わないことです。設計上は、少なくとも次の 3 つを区別して考える必要があります。

- 値がまだ存在しない
- 値は存在するが空である
- そのプロパティはこの文脈では意味を持たない

`strictNullChecks` を有効にすると、TypeScript はこの曖昧さを隠してくれなくなります。最初は面倒に感じますが、実務ではこの厳しさが効きます。`optional chaining` や `nullish coalescing` は便利ですが、それらで毎回ごまかしていると、本来どこで「ない」ことを扱うべきかが見えなくなります。

`nullability` は単なる文法ではなく、ドメインの設計です。ユーザーが未登録なのか、取得前なのか、権限不足で見えないのか、それとも値が空なのか。この違いを型で少しでも表現できると、実装もレビューもかなり楽になります。

### 2-5. `as const` とリテラル型の実務

TypeScript では、文字列や数値を単なる広い型ではなく、より具体的なリテラル型として扱えると便利な場面が多くあります。特に設定値、判別子、イベント名、状態遷移では有効です。

```ts
const status = "draft";
```

この `status` は文脈によって `"draft"` と推論される場合もありますが、オブジェクトや配列の中では広がって `string` になりやすいことがあります。`as const` を使うと、値をより固定的に扱えます。

```ts
const statuses = ["draft", "published", "archived"] as const;

type Status = (typeof statuses)[number];
```

この書き方を使うと、実際の値の一覧と型の定義をずらさずに保てます。実務では、設定ファイルのキー、画面状態、許可されたコマンド名などに向いています。

ただし、`as const` も乱用すると読みにくくなります。すべてを極端に狭い型へ固定したいわけではありません。「変わってほしくない値」「列挙可能な選択肢」「判別子として使う値」に限定して使うのが実務的です。

リテラル型は、型パズルの道具ではなく、曖昧な文字列を意味のある選択肢へ変える道具です。これを意識すると、API の引数、状態管理、イベント設計の見通しが良くなります。

## `TaskHub` で見る型システムの土台

ここまでの話を、通しサンプル `TaskHub` に引きつけて整理すると、次のようになります。

```ts
const taskStatuses = ["open", "done"] as const;

type TaskStatus = (typeof taskStatuses)[number];

type Task = {
  id: string;
  title: string;
  status: TaskStatus;
  dueAt: string | null;
};

function parseTaskStatus(value: unknown): TaskStatus | undefined {
  if (typeof value !== "string") return undefined;

  return taskStatuses.includes(value as TaskStatus)
    ? (value as TaskStatus)
    : undefined;
}
```

この短い例の中に、この章の基本要素がまとまっています。

- `taskStatuses` から `TaskStatus` を作り、文字列の選択肢を曖昧にしない
- `dueAt` を `string | null` にして、「ない」状態を明示する
- 外部入力は `unknown` から始め、使う前に確認する
- `Task` のような内部モデルは、以降の章で Node.js 側でもフロントエンド側でも共有の前提になる

この段階ではまだ小さな型定義に見えるかもしれません。しかし 4章以降では、この「内部で信頼できる型を先に作る」という考え方が、境界設計、モジュール設計、テスト戦略まで広がっていきます。
