---
created_by: cursor
created_at: 2026-04-04
project: TBP-J
task: プレビュー用ブランチのリモート push と GitHub Pages の手順
---

# 第2回スライド：リモート push とブラウザでの確認

**公開用リポジトリ:** [github.com/Shouheyhey/tbpj-us-trust-impact-session2](https://github.com/Shouheyhey/tbpj-us-trust-impact-session2)

ワークスペースからこのリポジトリへ同期する場合は、認証が効く**手元のターミナル**で次を実行するのが簡単です。

```bash
cd "/Users/shoheihayashi/Library/Mobile Documents/com~apple~CloudDocs/業務統合ワークスペース"
./scripts/sync-tbpj-session2-public-repo.sh
```

（SSH を使う場合: `export REPO_URL=git@github.com:Shouheyhey/tbpj-us-trust-impact-session2.git` を前置）

---

以下は **業務統合ワークスペース単体** に `origin` を付けてブランチだけ push する場合のメモです。

## 方針 A：ワークスペース全体のリポジトリにブランチだけ push（後で main にマージ）

1. GitHub で **空のリポジトリ**を作成（README なし推奨で競合が少ない）。
2. ターミナルで（`<URL>` を実際の HTTPS または SSH URL に置き換え）:

```bash
cd "/Users/shoheihayashi/Library/Mobile Documents/com~apple~CloudDocs/業務統合ワークスペース"
git checkout feature/tbp-session2-tbpp-revision
chmod +x scripts/push-tbp-session2-branch.sh
./scripts/push-tbp-session2-branch.sh "<URL>"
```

3. GitHub 上で **Pull Request**: `feature/tbp-session2-tbpp-revision` → `main`（または `master`）。承認後にマージすると「マスターに統合」が完了します。

### GitHub Pages で「このフォルダだけ」を見たい場合

Pages の「ルート」はリポジトリ直下の `index.html` です。本ワークスペースはルートに `index.html` がないため、**そのままではトップ URL でスライドは開きません。**

- **プレビュー URL の例**（ブランチを同じにした場合）:  
  `https://<ユーザー>.github.io/<リポジトリ>/03_TBP-J/outputs/public/web/us-trust-impact-session2/index_tbpp-revision.html`  
  ※ GitHub Pages を **Project site / 同じリポジトリ** で有効にし、ソースを **`feature/tbp-session2-tbpp-revision` ブランチの `/(root)`** にしたときのパスイメージです。実際のパスはリポジトリ構造に依存します。
- もっと簡単にするには **方針 B** を推奨します。

## 方針 B：スライド用の小さな public リポジトリだけ作る（README の「GitHub へ載せる手順」に近い）

1. GitHub で **新規 public リポジトリ**を作成（空）。
2. ローカルで **このフォルダの中身だけ**をコミットしたブランチを push:

```bash
cd "$(mktemp -d)"
git init -b feature/tbp-session2-tbpp-revision
cp -R "/Users/shoheihayashi/Library/Mobile Documents/com~apple~CloudDocs/業務統合ワークスペース/03_TBP-J/outputs/public/web/us-trust-impact-session2/"* .
git add .
git commit -m "Preview: session2 slides (tbpp-revision branch)"
git remote add origin "<スライド専用リポジトリのURL>"
git push -u origin feature/tbp-session2-tbpp-revision
```

3. GitHub → **Settings → Pages** → Branch: **`feature/tbp-session2-tbpp-revision`** / folder **`/(root)`** を保存。
4. 数分後、ブラウザで開く:  
   **`https://<ユーザー>.github.io/<リポジトリ名>/index_tbpp-revision.html`**

後から main に統合するときは、GitHub で `feature/...` → `main` にマージし、Pages のブランチを `main` に切り替えればよいです。

## 認証

- **HTTPS**: `git push` 時に Personal Access Token（classic の `repo`）をパスワードとして入力、または macOS キーチェーンに保存。
- **SSH**: `git@github.com:USER/REPO.git` と `ssh-keygen` / GitHub に公開鍵登録。
