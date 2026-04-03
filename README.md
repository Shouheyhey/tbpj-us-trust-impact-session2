---
created_by: cursor
created_at: 2026-04-03
project: TBP-J
task: README for public session2 HTML slides (GitHub Pages bundle)
---

# 第2回セミナー用スライド（静的HTML）

単一ファイルのスライドです。ビルド工程はありません。文面やスタイルの微修正は、Cursor でこのフォルダ内の `index.html` を直接編集してください。

## 構成

| ファイル | 説明 |
| --- | --- |
| `index.html` | スライド本体（GitHub Pages ではルートの `index.html` として配置） |
| `assets/trust-based-philanthropy-japan-logo.png` | Trust-Based Philanthropy Japan ロゴ |

## ローカルで確認

`index.html` をブラウザで開く。矢印キーまたは画面右下のボタンでスライド送り。

## GitHub へ載せる手順（独立の小さな public リポジトリ向け）

1. GitHub で新規の **public** リポジトリを作成する。
2. **このフォルダの中身だけ**（`index.html`, `assets/`, `README.md`）をクローン先のルートにコピーする。
3. コミットして `main` に push する。
4. リポジトリの Settings → Pages で、Branch `main` / フォルダ `/ (root)` を選び保存する。
5. 公開後も修正する場合は、ワークスペースで `index.html` を編集し、同じファイルをリポジトリ側に上書きコピーして再度 push する。

（ワークスペース全体の Git リポジトリをそのまま public にしないでください。）

## ロゴの取り扱い

組織ロゴの利用・再配布は社内のブランド／法務の取り決めに従ってください。
