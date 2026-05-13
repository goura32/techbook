# Status

## 全体

- 状態: 発行日を除き Fix 済み
- 最終更新: 2026-05-13
- 次のアクション: 発行日確定後に組版作業へ進む

## 章ごとの状態

| 章 | 状態 | メモ |
| --- | --- | --- |
| 1 | 本文推敲中 | `TaskHub` 導入済み |
| 2 | 本文推敲中 | `TaskHub` で型の土台を補強 |
| 3 | 本文推敲中 | `TaskHub` で API 契約を補強 |
| 4 | 本文推敲中 | 境界設計を `TaskHub` へ適用済み |
| 5 | 本文推敲中 | package 境界を `TaskHub` へ適用済み |
| 6 | 本文推敲中 | Node.js 実装を `TaskHub` へ適用済み |
| 7 | 本文推敲中 | frontend 実装を `TaskHub` へ適用済み |
| 8 | 本文推敲中 | テストと CI を `TaskHub` へ適用済み |
| 9 | 本文推敲中 | `tsconfig` 構成を `TaskHub` へ適用済み |
| 10 | 本文推敲中 | 時点明示と移行観点を補強済み |
| 11 | 本文推敲中 | 実務チェックリスト追加済み |

## 制作メモ

- 図表候補: [figures.md](/Users/goura32/techbook/books/typescript-practical/figures.md)
- 図表ラフ: [figures-roughs.md](/Users/goura32/techbook/books/typescript-practical/figures-roughs.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/typescript-practical/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/typescript-practical/code-samples.md)
- `TaskHub` 付録案: [appendix-taskhub-structure.md](/Users/goura32/techbook/books/typescript-practical/appendix-taskhub-structure.md), [appendix-taskhub-code-map.md](/Users/goura32/techbook/books/typescript-practical/appendix-taskhub-code-map.md)
- `TaskHub` 代表断片: [appendix-taskhub-core-fragments.md](/Users/goura32/techbook/books/typescript-practical/appendix-taskhub-core-fragments.md)
- レイアウト確認メモ: [layout-review.md](/Users/goura32/techbook/books/typescript-practical/layout-review.md)
- 原稿全体構成: [manuscript-structure.md](/Users/goura32/techbook/books/typescript-practical/manuscript-structure.md)
- 最終整形チェック: [final-assembly-checklist.md](/Users/goura32/techbook/books/typescript-practical/final-assembly-checklist.md)
- 参考情報候補: [reference-sources.md](/Users/goura32/techbook/books/typescript-practical/reference-sources.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/typescript-practical/reference-policy.md)
- 前付案: [front-matter.md](/Users/goura32/techbook/books/typescript-practical/front-matter.md)
- 後付案: [back-matter.md](/Users/goura32/techbook/books/typescript-practical/back-matter.md)
- 図表差し込み済み章: 1章, 4章, 5章, 8章, 9章, 10章
- 図表清書下書き済み: 図1-2, 図4-1, 図5-1, 図8-1, 図9-1, 表4-1, 表5-1, 表8-1, 表9-1, 表10-1, 図10-1
- `TaskHub` 付録本文: 初稿あり
- `TaskHub` 代表断片付録: 初稿あり
- 本文から付録への導線: 1章, 4章, 5章, 6章, 7章, 9章で確認済み
- 本文中の付録参照: ファイルリンクから `付録 A` `付録 B` 表記へ調整済み
- フロントエンド章の方針: フレームワーク固有 API より画面モデルと境界設計を優先する方針で固定済み
- 検証ライブラリの方針: 手書き検証を主役にし、`zod` は4章で代表例として短く補足する方針で固定済み
- Node.js 章の方針: ESM を主例にし、CJS は移行と互換性の注意として扱う方針で固定済み
- 10章の方針: 章頭で時点を固定し、本文では 6.0 と 7.0 Beta の転換点だけ日付付きで扱う方針で固定済み
- `TaskHub` の状態モデル: `open` / `done` へ統一済み
- 本文中の制作メモ: 各章から除去済み
- サンプル表示文言: 日本語基準へ調整済み
- 前付・後付の初稿: 追加済み
- 組版直前チェックリスト: 追加済み
- 目次粒度の方針: 前付と原稿全体構成へ反映済み
- 参考情報の転記方針: 後付と原稿全体構成へ反映済み
- 参考情報の最終候補: 別ファイルへ整理済み
- 参考情報欄: URL を含む転記下書きまで反映済み
- 参考情報の制作メモ: 別ファイルへ分離済み
- 奥付: 仮埋め済み
- 公開名: `大島 のりあ`
- 連絡先方針: メールを第一候補、SNS を補助候補とする
- 連絡先メールアドレス: `noria.library@gmail.com`
- 発行日: `2026年5月13日` を仮設定
- 周辺ツールの参考情報: `ESLint` `Vitest` `Playwright` を本書の掲載候補として維持し、それ以上は現時点で追加不要

## 未解決課題
