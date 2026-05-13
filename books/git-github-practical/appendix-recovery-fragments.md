# Appendix C. 代表的な運用断片と復旧断片

## この付録の役割

付録 A は `BeaconBoard` の構造、付録 B は操作と機能の対応を整理するためのものでした。この付録 C では、本文で断片的に登場した運用フローや復旧フローを、もう少し連続した形で確認できるようにします。

## 1. 小さな変更を Pull Request へ流す

```bash
git switch -c feature/update-docs-link
git add docs/architecture.md
git commit -m "docs: fix architecture guide links"
git push -u origin feature/update-docs-link
```

ここで重要なのは、変更量を小さく保っていることです。レビューしやすい差分を作ることが、後段の運用を楽にします。

## 2. main へ追従してから差分を整える

```bash
git fetch origin
git rebase origin/main
```

これは公開前の feature branch で使う分には自然な操作です。一方で、すでに共有ブランチとして使われている履歴へ同じことを行うと危険になります。

## 3. 誤ったマージを共有履歴上で打ち消す

```bash
git revert <merge-commit-sha>
git push origin main
```

共有ブランチで壊してしまったときは、まず `revert` を優先するほうが安全です。履歴を書き換えて見た目をきれいにするより、共同作業者が追える形で戻すほうが重要です。

## 4. hotfix へ必要なコミットだけを取り込む

```bash
git switch release/1.4
git cherry-pick <fix-commit-sha>
git push origin release/1.4
```

`cherry-pick` は便利ですが、文脈まで一緒に運んでくれるわけではありません。なぜそのコミットだけを取るのかを、PR や release notes で補う必要があります。

## 5. merge queue を前提にした流れ

1. Pull Request を作る
2. レビューと必須チェックを通す
3. queue へ入れる
4. `merge_group` の CI を通す
5. main へマージされる

この流れでは、個々の PR が単体でグリーンでも、まとめて main へ入る直前の状態で再確認される点が重要です。
