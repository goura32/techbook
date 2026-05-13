# Figures Plan

## 目的

- 文章だけだと追いにくい設計判断を、図表で短く伝える
- `TaskHub` の通しサンプルを読者が見失わないようにする
- KDP 向けの本文で、章ごとの要点が視覚的にも残るようにする

## 採用方針

- 図は「概念の整理」ではなく「判断軸の可視化」に使う
- 1 章あたり 0 から 2 点を目安にし、図がないと理解しにくい箇所だけに絞る
- 画面キャプチャよりも、長期的に陳腐化しにくい構造図、フロー図、比較表を優先する
- フレームワーク固有の UI 図は増やしすぎない

## 図表候補

### 1章

- 図: JavaScript の柔軟さが事故へ変わる流れ
  - 値の流入元
  - 暗黙の前提
  - 実行時エラー
  - TypeScript による前提の明示
  - 差し込み位置: 1-1 の導入段落直後
  - キャプション案: `JavaScript の柔軟さが、実務では暗黙の前提を増やし、実行時エラーへつながる流れ`
- 図: `TaskHub` 全体構成
  - `shared`
  - `api-client`
  - `cli`
  - `web`
  - 差し込み位置: 1-5 の構成説明直後
  - キャプション案: `本書を通して扱う TaskHub の全体構成`

### 2章

- 表: `unknown` `any` `never` の使い分け
  - 意味
  - 使いどころ
  - 避けたい誤用
- 図: 生の入力から内部型へ至る最小フロー
  - `unknown`
  - 確認
  - `TaskStatus`

### 3章

- 表: 関数契約で明示すべきもの
  - 入力
  - 出力
  - 失敗
  - オプション
- 図: `TaskHub` の API 契約
  - `FetchTasksInput`
  - `FetchTasksResult`
  - 呼び出し側の分岐

### 4章

- 図: 外部 API から内部モデルへの変換
  - `TaskApiResponse`
  - 検証
  - `toTask()`
  - `Task`
  - 差し込み位置: 4-3 の `toTask()` 説明直後
  - キャプション案: `TaskHub における外部 API レスポンスから内部モデルへの変換`
- 表: 境界ごとの壊れ方
  - API
  - フォーム
  - 環境変数
  - CLI 引数
  - 差し込み位置: 4-2 の 3 段階説明直後
  - キャプション案: `境界ごとに起きやすい壊れ方の比較`

### 5章

- 図: `TaskHub` の package 境界
  - `packages/shared`
  - `packages/api-client`
  - `apps/cli`
  - `apps/web`
  - 差し込み位置: 5-1 の `shared` / `TaskApiResponse` 説明直後
  - キャプション案: `TaskHub の package 境界と依存方向`
- 表: 公開 API と非公開実装の分け方
  - 差し込み位置: 5-2 の `toTask()` `fetchTasks()` 説明直後
  - キャプション案: `公開 API と非公開実装を分ける判断基準`

### 6章

- 図: Node.js CLI 側の責務分担
  - 設定読み込み
  - API 呼び出し
  - 表示
  - 終了コード

### 7章

- 図: API クライアントから UI 表示までの境界
  - `Task`
  - `toTaskListItem()`
  - `TaskListItem`
  - Component

### 8章

- 図: テスト責務の分担
  - 型検査
  - 単体テスト
  - 統合テスト
  - E2E
  - 差し込み位置: 8-1 の `TaskHub` における役割分担直後
  - キャプション案: `TaskHub における型検査、単体テスト、統合テスト、E2E の役割分担`
- 表: `TaskHub` に対する CI ジョブ構成
  - 差し込み位置: 8-5 の CI YAML 説明直後
  - キャプション案: `TaskHub の CI ジョブ構成と責務`

### 9章

- 図: `tsconfig` と `project references` の関係
  - root
  - `shared`
  - `api-client`
  - `cli`
  - `web`
  - 差し込み位置: 9-3 の root `tsconfig.json` 例直後
  - キャプション案: `TaskHub における root tsconfig と project references の関係`
- 表: `tsconfig` の主要オプションと責務
  - 差し込み位置: 9-5 の `packages/shared/tsconfig.json` 例直後
  - キャプション案: `主要な tsconfig オプションと責務`

### 10章

- 表: 2026年5月12日時点の TypeScript バージョン状況
  - 5.9
  - 6.0
  - 7.0 Beta
  - native preview
  - 差し込み位置: 10-2 の公式状況整理直後
  - キャプション案: `2026年5月12日時点の TypeScript バージョン状況`
- 図: 移行判断フロー
  - 現状確認
  - 互換性確認
  - 試験導入
  - 本採用
  - 差し込み位置: 10-4 の移行計画説明末尾
  - キャプション案: `TypeScript 更新時の移行判断フロー`

### 11章

- 表: 継続運用チェックリスト
  - 更新前
  - レビュー時
  - 型負債返済時

## 優先順位

1. 1章 `TaskHub` 全体構成
2. 4章 外部 API から内部モデルへの変換
3. 5章 `TaskHub` の package 境界
4. 8章 テスト責務の分担
5. 9章 `project references` 図
6. 10章 バージョン状況表

## 推敲時の確認

- 図が本文の繰り返しになっていないか
- 図が古くなりやすいツール名や画面に依存しすぎていないか
- 図を見たあとに本文へ戻ったとき、判断軸が明確になるか
