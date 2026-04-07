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

### ターミナルで「正しいパスワード」を入れても push できないとき

GitHub は **Git 用にアカウントのログインパスワードを受け付けません**（2011年以降の方針、2021年以降は PAT か SSH が必要）。

**いちばん早い回避（PAT を1行だけ環境変数に載せる）**

1. ブラウザで GitHub にログイン → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)** → **Generate new token (classic)**。
2. **repo** にチェック → 生成したトークン（`ghp_...`）をコピー（再表示されないので必ず保存）。
3. **ターミナル.app**（Cursor 内でも可）で、**同じシェルで**次を続けて実行（トークンは他人に見せない／チャットに貼らない）。

```bash
export GITHUB_TOKEN='ghp_ここに貼り付け'
cd "/Users/shoheihayashi/Library/Mobile Documents/com~apple~CloudDocs/業務統合ワークスペース"
chmod +x scripts/sync-tbpj-session2-public-repo.sh
./scripts/sync-tbpj-session2-public-repo.sh main
unset GITHUB_TOKEN
```

`sync-tbpj-session2-public-repo.sh` は **`GITHUB_TOKEN` が入っているときだけ**、push 時にそのトークンで HTTPS 認証します（clone は従来どおり公開 URL）。スクリプトは **`x-access-token` ＋ PAT** 形式で push します（`oauth2:` より通りやすいことが多いです）。

### `Invalid username or token. Password authentication is not supported` が出るとき

1. **GitHub の「サイト用パスワード」を入れていないか確認する。** 通るのは **PAT（`ghp_` で始まる文字列）** だけです。
2. **トークンの種類**
   - **Classic:** 作成時に **`repo`** にチェックが付いているか。
   - **Fine-grained:** リポジトリ `tbpj-us-trust-impact-session2` を選び、**Contents: Read and write**（および必要なら **Metadata**）が付いているか。
3. **コピペの余計な空白・改行**で失敗することがあります。トークンは先頭 `ghp_` から末尾まで、**余分なスペースなし**で `export GITHUB_TOKEN='...'` に入れる。
4. **macOS が古いパスワードをキーチェーンから勝手に送っている**場合、PAT を入れても上書きされず同じエラーになります。ターミナルで次を実行してから、もう一度 `GITHUB_TOKEN` 付きで同期スクリプトを実行する。

```bash
printf 'protocol=https\nhost=github.com\n\n' | git credential-osxkeychain erase
```

5. それでもダメなら、**GitHub ユーザー名を明示**して push する（classic PAT 向け）。

```bash
export GITHUB_USERNAME='あなたのGitHubのユーザー名'
export GITHUB_TOKEN='ghp_...'
./scripts/sync-tbpj-session2-public-repo.sh main
unset GITHUB_TOKEN GITHUB_USERNAME
```

**キーチェーンに「古い誤パスワード」が残っている場合**

- **キーチェーンアクセス**で `github.com` を検索し、該当項目を削除してから、もう一度 `git push` で PAT を入力する。  
  または上記の **`GITHUB_TOKEN` 方式**ならプロンプトを経由しません。

**SSH で済ませたい場合**

- `ssh-keygen` で鍵を作成 → GitHub の **SSH keys** に公開鍵を登録 →  
  `REPO_URL=git@github.com:Shouheyhey/tbpj-us-trust-impact-session2.git ./scripts/sync-tbpj-session2-public-repo.sh main`
