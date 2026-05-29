# wws-publish スキル
## HTMLモックアップを staticrypt で暗号化して GitHub Pages に公開する

**対象**: wws-ws-saas-spec-html-internal / wws-ws-saas-hearing-pptx-public で生成した HTML 提案資料  
**目的**: クライアント・代理店向けにパスワード付きURLで安全共有する

---

## 概要フロー

```
HTMLファイル
  ↓ (1) staticrypt で暗号化
暗号化HTML（index.html）
  ↓ (2) GitHub リポジトリ作成 & push
GitHub Pages URL（公開）
  ↓ (3) URL + パスワードをクライアントに共有
```

---

## 前提条件

```bash
# GitHub CLI のインストール確認
gh --version

# Node.js の確認（staticrypt は npx で実行可）
node --version

# GitHub にログイン済みか確認
gh auth status
```

---

## STEP 1｜暗号化（staticrypt）

```bash
# 基本コマンド（インストール不要・npxで実行）
npx staticrypt@latest {input.html} {PASSWORD} -o index.html

# 例：プランB モックアップを暗号化
npx staticrypt@latest mockup_planb_transit_campaign.html Bizavi2026 -o index.html

# オプション
# --template-title "タイトル"   ← パスワード入力画面のタイトル
# --template-instructions "説明文"  ← 入力画面の説明
# --template-color-primary "#1b5e20"  ← ブランドカラー
# --remember 7  ← 7日間パスワードを記憶
```

### パスワードルール（推奨）
| 用途 | 命名例 |
|------|--------|
| コンペ提案（関係者のみ） | `Bizavi2026` |
| 内部レビュー | `WWS_Internal_XX` |
| クライアント共有 | クライアント名 + 年 |

---

## STEP 2｜GitHub リポジトリ作成 & push

```bash
# 作業ディレクトリを作成
mkdir {repo-name} && cd {repo-name}

# 暗号化済み index.html をコピー
copy ..\index.html index.html   # Windows
# cp ../index.html index.html   # Mac/Linux

# git 初期化 & 初回コミット
git init
git add index.html
git commit -m "add encrypted mockup"

# GitHub にリポジトリ作成 & push（公開リポジトリ = Pages 無料）
gh repo create {repo-name} --public --source=. --remote=origin --push

# GitHub Pages 有効化（main ブランチのルート）
gh api repos/{GITHUB_USER}/{repo-name}/pages \
  --method POST \
  --field source='{"branch":"main","path":"/"}' \
  -H "Accept: application/vnd.github.v3+json"
```

### リポジトリ命名規則
```
{案件キーワード}-{プラン}
例:
  transit-campaign-planb
  transit-campaign-plana
  entre-support-proposal
  map-penguin-demo
```

---

## STEP 3｜公開 URL の確認

```bash
# Pages のデプロイ状態を確認（1〜2分後）
gh api repos/{GITHUB_USER}/{repo-name}/pages

# URL は以下の形式
# https://{GITHUB_USER}.github.io/{repo-name}/
```

### 例
```
URL      : https://kamat-wws.github.io/transit-campaign-planb/
Password : Bizavi2026
```

---

## 自動化スクリプト（publish_html.sh / publish_html.bat）

`C:\Users\kamat\scripts\` に `publish_html.bat` として保存する。

```bat
@echo off
REM ============================================================
REM  wws-publish : HTML → staticrypt → GitHub Pages
REM  使い方: publish_html.bat {htmlファイル} {リポジトリ名} {パスワード}
REM  例    : publish_html.bat mockup_planb.html transit-planb Bizavi2026
REM ============================================================

SET HTML_FILE=%1
SET REPO_NAME=%2
SET PASSWORD=%3
SET GITHUB_USER=wwskamata

IF "%HTML_FILE%"=="" (echo 引数1: HTMLファイルが必要です & exit /b 1)
IF "%REPO_NAME%"=="" (echo 引数2: リポジトリ名が必要です & exit /b 1)
IF "%PASSWORD%"=="" (echo 引数3: パスワードが必要です & exit /b 1)

echo [1/5] staticrypt で暗号化中...
npx staticrypt@latest "%HTML_FILE%" "%PASSWORD%" ^
  --template-title "WWS 提案資料" ^
  --template-instructions "担当者よりお伝えしたパスワードを入力してください" ^
  --template-color-primary "#e87722" ^
  --remember 7 ^
  -o index.html
IF ERRORLEVEL 1 (echo ❌ 暗号化失敗 & exit /b 1)
echo ✅ 暗号化完了

echo [2/5] 作業ディレクトリ作成...
IF EXIST "%REPO_NAME%" rmdir /s /q "%REPO_NAME%"
mkdir "%REPO_NAME%"
copy index.html "%REPO_NAME%\index.html" >nul

echo [3/5] git 初期化 & コミット...
cd "%REPO_NAME%"
git init -q
git add index.html
git commit -q -m "add encrypted proposal"

echo [4/5] GitHub リポジトリ作成 & push...
gh repo create "%REPO_NAME%" --public --source=. --remote=origin --push
IF ERRORLEVEL 1 (echo ❌ GitHub push 失敗 & cd .. & exit /b 1)

echo [5/5] GitHub Pages 有効化...
gh api "repos/%GITHUB_USER%/%REPO_NAME%/pages" ^
  --method POST ^
  --field source="{\"branch\":\"main\",\"path\":\"/\"}" ^
  -H "Accept: application/vnd.github.v3+json" >nul 2>&1

cd ..

echo.
echo ============================================================
echo  ✅ 公開完了（1〜2分後に有効）
echo  URL      : https://%GITHUB_USER%.github.io/%REPO_NAME%/
echo  Password : %PASSWORD%
echo ============================================================
```

---

## 手動実行（バッチ不要の場合）

```bat
REM 例：公共交通キャンペーン プランBを公開
cd C:\Users\kamat\wws-local-project\assets

npx staticrypt@latest mockup_planb_transit_campaign.html Bizavi2026 ^
  --template-title "公共交通キャンペーン提案 プランB" ^
  --template-instructions "担当者よりお伝えしたパスワードを入力してください" ^
  --template-color-primary "#1b5e20" ^
  --remember 7 ^
  -o index.html
```

---

## 管理台帳（共有済みURL一覧）

`C:\Users\kamat\wws-local-project\publish_log.md` に記録する。

```markdown
| 日付 | 案件 | プラン | URL | パスワード | 有効期限 |
|------|------|--------|-----|-----------|---------|
| 2026-05-xx | 公共交通キャンペーン | プランB | https://... | Bizavi2026 | — |
```

---

## セキュリティ注意事項

| 項目 | 対応 |
|------|------|
| **コンテンツ保護** | staticrypt による AES-256 暗号化（クライアントサイド）|
| **リポジトリ** | public だが暗号化済みのため HTML 内容は読めない |
| **パスワード管理** | Slack DM または電話口で別途共有。メール本文に記載しない |
| **期限切れ対応** | リポジトリごと削除: `gh repo delete {name} --yes` |
| **上書き更新** | index.html を差し替えて `git push` するだけ |

---

## トラブルシューティング

| 症状 | 原因 | 対処 |
|------|------|------|
| Pages が404 | デプロイに時間がかかる | 2〜3分待って再アクセス |
| `gh auth` エラー | 未ログイン | `gh auth login` を実行 |
| staticrypt コマンドが遅い | npxの初回DL | 2回目以降は速い |
| パスワードが通らない | 文字コード問題 | 記号なしの英数字パスワードを使う |

---

## 参照コマンド（よく使う操作）

```bash
# リポジトリ一覧
gh repo list --limit 20

# Pages URL 確認
gh api repos/{USER}/{REPO}/pages | jq '.html_url'

# リポジトリ削除（公開停止）
gh repo delete {REPO_NAME} --yes

# index.html を更新して再push
git add index.html && git commit -m "update" && git push
```
