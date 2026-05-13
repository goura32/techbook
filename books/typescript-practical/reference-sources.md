# Reference Sources

## 目的

- `参考情報` 欄へ最終転記する候補を、本書で使った主張と対応づけて管理する
- KDP 用の後付へ転記するときに、リンク集ではなく本文に効いている情報だけを残す

## 優先候補

### TypeScript

1. TypeScript Handbook  
   種別: 公式ドキュメント  
   主な対応章: 1章, 2章, 3章  
   役割: 本書全体の型システムと言語理解の土台
   URL: `https://www.typescriptlang.org/docs/`

2. Modules - Reference  
   種別: 公式ドキュメント  
   主な対応章: 5章, 6章, 9章  
   役割: モジュール設計、`nodenext`、import/export の補助
   URL: `https://www.typescriptlang.org/docs/handbook/modules/reference`

3. Announcing TypeScript 6.0  
   種別: 公式ブログ  
   日付: 2026年3月23日  
   主な対応章: 10章  
   役割: 6.0 を 7.0 への橋渡しとして扱う根拠
   URL: `https://devblogs.microsoft.com/typescript/announcing-typescript-6-0/`

4. Announcing TypeScript 7.0 Beta  
   種別: 公式ブログ  
   日付: 2026年4月21日  
   主な対応章: 10章  
   役割: 7.0 Beta と native toolchain の位置づけ
   URL: `https://devblogs.microsoft.com/typescript/announcing-typescript-7-0-beta/`

### Node.js

5. Modules: Packages  
   種別: 公式ドキュメント  
   主な対応章: 6章, 9章  
   役割: `package.json` の `"type": "module"` と package 境界の前提
   URL: `https://nodejs.org/api/packages.html`

6. ECMAScript Modules  
   種別: 公式ドキュメント  
   主な対応章: 6章  
   役割: ESM 基準で Node.js を扱う前提の補助
   URL: `https://nodejs.org/api/esm.html`

## 転記時のルール

- 後付には 6 件前後を上限目安にする
- 本文で直接使っていないものは載せない
- 日付依存の強い項目だけ公開日を併記する
- URL は最終組版直前に確認して転記する

## 追加候補

7. ESLint  
   種別: 公式ドキュメント  
   主な対応章: 9章  
   役割: `tsconfig` と editor / lint の整合説明の補助  
   URL: `https://eslint.org/docs/latest/`

8. Vitest  
   種別: 公式ドキュメント  
   主な対応章: 8章  
   役割: CI と単体テストの具体例を補助  
   URL: `https://vitest.dev/guide/`

9. Playwright  
   種別: 公式ドキュメント  
   主な対応章: 8章  
   役割: E2E の具体例を補助  
   URL: `https://playwright.dev/docs/intro`

現時点では、周辺ツールの候補追加は不要とする。本文中で名前を挙げていても、主張の理解に直結しない `pnpm` `tsx` などは後付の参考情報へは載せない。
