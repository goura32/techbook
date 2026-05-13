# Code Samples Plan

## 結論

- `TaskHub` のコードは、章内には短い断片を残す
- 全体像は別紙として管理する
- 本文では「その章で説明したい判断」に必要な最小コードだけを見せる

## この方針を採る理由

- 章内で全体コードを毎回追わせると、設計の説明より行数の把握に意識が向きやすい
- 逆に断片だけだと、読者が `TaskHub` の全体像を見失いやすい
- そのため、本文は局所、別紙は全体という分担にする

## 本文での扱い

- 1 つのコード例は、原則として 10 行から 30 行程度に収める
- 章の主題と直接関係しない補助コードは省略する
- 断片の前後で、「このコードは `TaskHub` のどこにあるか」を短く示す
- 同じコードを再掲する場合は、改善点が分かる範囲だけ抜き出す

## 別紙で持つもの

- `TaskHub` の想定ディレクトリ構成
- `shared` の主要型
- `api-client` の主要変換関数
- `cli` の設定読み込みと起動入口
- `web` の view model と主要コンポーネント境界
- 代表断片をひと続きで見直せる付録

## 章との対応

- 1章: `TaskHub` 全体像だけ示す
- 2章: `TaskStatus` と `parseTaskStatus()`
- 3章: `FetchTasksInput` `FetchTasksResult` `ApiResult`
- 4章: `TaskApiResponse` と `toTask()`
- 5章: package 構成と公開 API の断片
- 6章: `TaskHubConfig` `loadTaskHubConfig()` `fetchTasks()`
- 7章: `TaskListItem` `toTaskListItem()`
- 8章: テスト対象関数と CI 断片
- 9章: root `tsconfig.json` と package ごとの設定断片
- 10章: 実コードより設定と更新判断を優先
- 11章: コードより運用チェックリストを優先

## 別紙化する候補ファイル

- `appendix-taskhub-structure.md`
- `appendix-taskhub-code-map.md`
- `appendix-taskhub-core-fragments.md`

## 推敲時の確認

- コード例の長さが、本文の主張より目立っていないか
- 断片だけ読んでも、その章の判断軸が伝わるか
- 別紙なしでも本文の流れは追えるか
- 別紙があれば、読者が `TaskHub` 全体へ戻れるか
