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
| `index_tbpp-revision.html` | **共同代表FB反映稿**。`index.html` と内容を同期済みの場合あり。 |
| `assets/trust-based-philanthropy-japan-logo.png` | Trust-Based Philanthropy Japan ロゴ |
| `assets/tbp-2025-04-09-pdf/page-01.png` … `page-09.png` | **TBPP配布PDF**（`2025-04-09 TBP.pdf`）全9ページを PyMuPDF でレンダリングした画像 |
| `assets/tbpp-in-4d-official-diagram.png` 等 | 旧ppt抽出の残置（参照用。本デッキの主参照は上記PDF画像） |

PDFを差し替えたときは、同解像度で `page-NN.png` を再生成し、HTML内の枚数とパスを合わせてください。

```text
pip install pymupdf
python -c "import fitz; from pathlib import Path; pdf=Path(r'…\2025-04-09 TBP.pdf'); out=Path(r'…\assets\tbp-2025-04-09-pdf'); out.mkdir(parents=True, exist_ok=True); d=fitz.open(pdf); m=fitz.Matrix(2,2); [d.load_page(i).get_pixmap(matrix=m,alpha=False).save(out/f'page-{i+1:02d}.png') for i in range(len(d))]; d.close()"
```

## ローカルで確認

`index.html` をブラウザで開く。矢印キーまたは画面右下のボタンでスライド送り。

## GitHub へ載せる手順（独立の小さな public リポジトリ向け）

1. GitHub で新規の **public** リポジトリを作成する。
2. **このフォルダの中身だけ**（`index.html`, `assets/`, `README.md`）をクローン先のルートにコピーする。
3. コミットして `main` に push する。
4. リポジトリの Settings → Pages で、Branch `main` / フォルダ `/ (root)` を選び保存する。
5. 公開後も修正する場合は、ワークスペースで `index.html` を編集し、同じファイルをリポジトリ側に上書きコピーして再度 push する。

（ワークスペース全体の Git リポジトリをそのまま public にしないでください。）

## プレビュー用ブランチのリモート push / GitHub Pages

`feature/tbp-session2-tbpp-revision` をリモートに載せてブラウザで確認し、後から `main` にマージする手順は [PUSH_AND_PAGES.md](PUSH_AND_PAGES.md) を参照。ワークスペースルートで `./scripts/push-tbp-session2-branch.sh '<リモートURL>'` も利用可（`origin` 未設定時のみ `origin` を追加）。

## ロゴの取り扱い

組織ロゴの利用・再配布は社内のブランド／法務の取り決めに従ってください。
