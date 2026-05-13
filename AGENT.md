# AGENT.md

このリポジトリは、複数の技術書を並行して執筆するためのモノレポです。

## 基本原則

1. 新しい原稿や改稿に着手する前に、必ず以下を確認する。
   - `shared/style-guide.md`
   - `shared/terminology.md`
   - `shared/book-template.md`
   - `shared/book-candidates.md`
   - `shared/series-title-candidates.md`
   - `shared/common-explanations.md`
   - 自分が担当する書籍以外も含む `books/*/outline.md`
   - 自分が担当する書籍以外も含む `books/*/status.md`
2. 複数の書籍で繰り返し登場する説明は、本文へ重複記述せず `shared/common-explanations.md` に寄せる。
3. 各書籍では、共通説明をそのまま再掲するのではなく、必要な文脈だけを簡潔に参照し、当該書籍固有の補足に集中する。
4. 用語、表記、コードスタイル、章構成は共通方針を優先し、各書籍固有の例外は `books/<book-id>/writing-policy.md` に明記する。
5. 各書籍の進捗は `books/<book-id>/status.md` で管理し、章ごとの状態が分かるように更新する。

## 新しい書籍を追加する手順

1. `books/_template/` を複製して `books/<book-id>/` を作成する。
2. `outline.md` に章構成案を書く。
3. `target-reader.md` に想定読者、前提知識、到達目標を書く。
4. `writing-policy.md` に書籍固有の方針や制約を書く。
5. `status.md` に執筆状況を記録する。
6. 本文を書き始める前に、既存書籍との重複がないかを確認し、必要なら `shared/common-explanations.md` を更新する。

## ディレクトリ方針

- `shared/`: 全書籍で共有する方針、用語、テンプレート、共通説明
- `shared/book-candidates.md`: 書籍候補、企画メモ、優先度、差別化案
- `shared/series-title-candidates.md`: シリーズ名候補と命名方針
- `books/<book-id>/`: 各書籍の設計情報と本文
- `books/<book-id>/chapters/`: 章本文、付録、図表メモなど

## レビュー観点

- 読者層と章構成が一致しているか
- 他書と説明の棲み分けができているか
- 共通説明へ抽出すべき重複が残っていないか
- 用語と表記が共通ガイドと一致しているか
- `status.md` が実態に追従しているか
