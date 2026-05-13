# Manuscript Structure

## 目的

- KDP 向けに、前付・本文・付録・後付を含めた全体構成を固定する
- 執筆用ファイル群と、組版用原稿の並びを対応づける
- 最終整形時に不足要素を見失わないようにする

## 全体構成

1. 表紙
2. 扉
3. はじめに
4. 目次
5. 本文 1章から11章
6. 付録 A から C
7. おわりに
8. 参考情報
9. 著者紹介
10. 奥付

## 組版用の並び

### 前付

1. 扉
2. はじめに
3. 本書の読み方
4. 目次

### 本文

1. 1章 `Git と GitHub をどう使い分けるか`
2. 2章 `コミット、履歴、差分の基本設計`
3. 3章 `ブランチ戦略と日常運用`
4. 4章 `Pull Request とレビューの進め方`
5. 5章 `競合解消、rebase、cherry-pick の実務`
6. 6章 `Rulesets、保護設定、権限設計`
7. 7章 `Merge Queue と CI の実務`
8. 8章 `Issue、Projects、Discussions の使い分け`
9. 9章 `リリース、タグ、バージョニング`
10. 10章 `事故対応と復旧`
11. 11章 `チームに定着させる運用ルール`

### 付録

1. 付録 A `BeaconBoard の全体構成`
2. 付録 B `主要操作と機能の対応`
3. 付録 C `代表的な運用断片と復旧断片`

### 後付

1. おわりに
2. 参考情報
3. 著者紹介
4. 奥付

## ファイル対応

### 本文

- 1章: [01-git-and-github-in-practice.md](/Users/goura32/techbook/books/git-github-practical/chapters/01-git-and-github-in-practice.md)
- 2章: [02-commits-history-and-diffs.md](/Users/goura32/techbook/books/git-github-practical/chapters/02-commits-history-and-diffs.md)
- 3章: [03-branching-strategies-and-daily-flow.md](/Users/goura32/techbook/books/git-github-practical/chapters/03-branching-strategies-and-daily-flow.md)
- 4章: [04-pull-requests-and-review.md](/Users/goura32/techbook/books/git-github-practical/chapters/04-pull-requests-and-review.md)
- 5章: [05-conflicts-rebase-and-cherry-pick.md](/Users/goura32/techbook/books/git-github-practical/chapters/05-conflicts-rebase-and-cherry-pick.md)
- 6章: [06-rulesets-protection-and-permissions.md](/Users/goura32/techbook/books/git-github-practical/chapters/06-rulesets-protection-and-permissions.md)
- 7章: [07-merge-queue-and-ci.md](/Users/goura32/techbook/books/git-github-practical/chapters/07-merge-queue-and-ci.md)
- 8章: [08-issues-projects-and-discussions.md](/Users/goura32/techbook/books/git-github-practical/chapters/08-issues-projects-and-discussions.md)
- 9章: [09-releases-tags-and-versioning.md](/Users/goura32/techbook/books/git-github-practical/chapters/09-releases-tags-and-versioning.md)
- 10章: [10-incident-response-and-recovery.md](/Users/goura32/techbook/books/git-github-practical/chapters/10-incident-response-and-recovery.md)
- 11章: [11-making-workflows-stick.md](/Users/goura32/techbook/books/git-github-practical/chapters/11-making-workflows-stick.md)

### 付録

- 付録 A: [appendix-beaconboard-structure.md](/Users/goura32/techbook/books/git-github-practical/appendix-beaconboard-structure.md)
- 付録 B: [appendix-workflow-map.md](/Users/goura32/techbook/books/git-github-practical/appendix-workflow-map.md)
- 付録 C: [appendix-recovery-fragments.md](/Users/goura32/techbook/books/git-github-practical/appendix-recovery-fragments.md)

### 制作支援

- 図表計画: [figures.md](/Users/goura32/techbook/books/git-github-practical/figures.md)
- 図表ラフ: [figures-roughs.md](/Users/goura32/techbook/books/git-github-practical/figures-roughs.md)
- 図表清書下書き: [figures-final.md](/Users/goura32/techbook/books/git-github-practical/figures-final.md)
- コード掲載方針: [code-samples.md](/Users/goura32/techbook/books/git-github-practical/code-samples.md)
- レイアウト確認: [layout-review.md](/Users/goura32/techbook/books/git-github-practical/layout-review.md)
- 参考情報方針: [reference-policy.md](/Users/goura32/techbook/books/git-github-practical/reference-policy.md)

## 章間のつなぎ

- はじめに:
  - 本書の目的
  - 想定読者
  - `BeaconBoard` を使う理由
  - 読み方の案内
- 本文 1章から5章:
  - Git の土台と日常運用編
- 本文 6章から9章:
  - GitHub の保護設定、CI、リリース編
- 本文 10章から11章:
  - 復旧と定着編
- おわりに:
  - 本書全体の視点を短く回収する

## 未着手の要素

- 扉の最終表記
- 参考情報の URL / 版情報の最終転記
- 目次の最終粒度決定

現時点では、奥付は仮埋め済みで、発行日は 2026年5月13日を仮設定として最終整形を進める前提とする。

## 組版前チェック

- 章タイトル表記が目次と本文で一致しているか
- 付録 A / B / C の参照表記が本文と一致しているか
- 図表番号と図表キャプションが本文順に並ぶか
- 前付と後付を入れたあとでページバランスが崩れないか

詳細な作業チェックは [final-assembly-checklist.md](/Users/goura32/techbook/books/git-github-practical/final-assembly-checklist.md) を使う。

## 目次粒度の採用方針

- 基本は 1 段階目の見出しのみ
- 章数が多いため、まずは一覧性を優先する
- 節見出しを追加する場合は、長い章だけへ限定する
- 付録と後付の主要見出しは目次へ含める

## 参考情報の最終転記方針

- 公式情報を優先する
- 本文で直接使った判断材料を優先する
- 時点依存の強い項目だけ必要に応じて扱う
- URL は組版直前に一括で点検して転記する

転記候補の管理は [reference-sources.md](/Users/goura32/techbook/books/git-github-practical/reference-sources.md) を使う。
