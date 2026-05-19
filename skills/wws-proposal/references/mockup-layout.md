# モックアップレイアウト リファレンス
# ⚠️ このファイルに記載のルールは必ず守ること（確定済みレイアウト規約）

---

## 🔒 レイアウト確定ルール（変更禁止）

### 全体構成（3段固定）
```
① タイトルバー    height:44px 固定・1カラム全幅
② ユーザーフロー図 固定高・1カラム全幅・横スクロール対応
③ 左右分割エリア  HTMLテーブルレイアウト（table-layout:fixed）
   ├── 左（66.6%）：画面フロー  縦1列・中央揃え・独立スクロール
   └── 右（33.4%）：工数算定依頼テーブル・独立スクロール
```

### ⚠️ 禁止事項（破ると表示が崩れる）

| NG | 理由 |
|----|------|
| `display:flex` で左右分割する | iframeで崩れる |
| `height:calc()` で td-inner の高さ固定する | スクロールが途中で切れる |
| `height:100vh` / `position:fixed` を使う | iframeで機能しない |
| 左右を逆（工数左・画面右）にする | 見せ方の確定ルール違反 |
| `.grid` に `flex-wrap:wrap` を使う | 横並びになる |
| flexbox で左右並列を実現しようとする | 環境依存で必ず崩れる |
| td-left の幅を%指定にする | phoneフレームの右側に大きな余白が出る |

### ✅ 必須ルール

| ルール | 内容 |
|--------|------|
| 左右並列は必ず `<table>` + `table-layout:fixed` | flexbox禁止 |
| スクロールは `overflow-y:auto` のみ。height固定しない | calc禁止 |
| 左=画面フロー、右=工数算定。順序固定 | 順序固定 |
| 画面フロー列（td-left）の幅は `width:320px` 固定（phone280px+余白） | %指定禁止 |
| td-left の左右padding は `8px` 最小余白 | 余白節約 |
| 工数算定列（td-right）は幅指定なし（残り幅を自動で使う） | 幅最大化 |
| 画面は縦1列・中央揃え（`flex-direction:column; align-items:center`） | wrap禁止 |
| バッジ（F-01/B-01等）は各画面phoneフレームの右上にオーバーレイ表示 | 視認性確保 |
| バッジ色は `.id-cust/.id-std/.id-reci/.id-ops` を使う | 色統一 |
| カスタム箇所は `.custom-mark` クラスで赤枠表示 | 視認性確保 |
| 各画面は必ず `screen-wrap > phone > phone-inner` の3階層で記述 | ネスト崩れ防止 |
| gridに追加する画面は必ず独立した `<div class="screen-wrap">` で開始する | 前画面への混入防止 |

---

## 確定CSSテンプレート（コピー必須・変更箇所以外は一字一句変えない）

```css
*{box-sizing:border-box;margin:0;padding:0;}
body{margin:0;padding:0;font-family:'Noto Sans JP',sans-serif;}

/* ① タイトルバー */
#titlebar{
  background:#fff;border-bottom:2px solid #e87722;
  padding:0 20px;height:44px;
  display:flex;align-items:center;justify-content:space-between;
  box-shadow:0 1px 4px rgba(0,0,0,.07);
  font-family:"Noto Sans JP",sans-serif;
}
#titlebar h1{font-size:13px;font-weight:700;color:#333;}
#titlebar p{font-size:9px;color:#888;margin-top:1px;}
.legend{display:flex;gap:12px;font-size:10px;align-items:center;}
.legend-item{display:flex;align-items:center;gap:4px;}
.lbox{width:11px;height:11px;border-radius:2px;flex-shrink:0;}
.lbox.c{border:2px solid #e53935;background:#fff5f5;}
.lbox.o{background:#fff;border:1px solid #ccc;}

/* ② フロー図バー */
#flowbar{
  background:#fff;border-bottom:2px solid #e0e0e0;
  padding:10px 20px 12px;
  font-family:"Noto Sans JP",sans-serif;
}
#flowbar .ftitle{
  font-size:12px;font-weight:700;color:#333;
  border-left:4px solid #e87722;padding-left:8px;margin-bottom:10px;
}

/* ③ テーブルレイアウト（flexbox禁止） */
#panels-table{
  width:100%;
  border-collapse:collapse;
  table-layout:fixed;                /* ← 必須。省略禁止 */
  font-family:"Noto Sans JP",sans-serif;
  font-size:12px;
}
#panels-table > tbody > tr > td{vertical-align:top;padding:0;}

/* 左セル：画面フロー 2/3 */
#td-left{width:66.6%;background:#f0f0f0;border-right:2px solid #e0e0e0;}
#td-left-inner{
  overflow-y:auto;           /* ← height固定しない。これだけでOK */
  padding:14px 12px 48px;   /* ← 下48pxで最終画面が隠れない */
}

/* 右セル：工数算定 1/3 */
#td-right{width:33.4%;background:#fff;}
#td-right-inner{
  overflow-y:auto;           /* ← height固定しない */
  padding:14px 16px 48px;
}

/* 画面グリッド：縦1列・中央揃え */
.grid{
  display:flex;
  flex-direction:column;    /* ← 縦1列固定 */
  align-items:center;       /* ← 中央揃え */
  gap:32px;
}

/* バッジ（①タイトルバー・②フロー図・③画面ラベルで共通） */
.id-cust{background:#fee2e2;color:#991b1b;border:1px solid #fca5a5;border-radius:99px;padding:2px 7px;font-size:9px;font-weight:700;white-space:nowrap;}
.id-std {background:#dcfce7;color:#166534;border:1px solid #86efac;border-radius:99px;padding:2px 7px;font-size:9px;font-weight:700;white-space:nowrap;}
.id-reci{background:#dbeafe;color:#1e40af;border:1px solid #93c5fd;border-radius:99px;padding:2px 7px;font-size:9px;font-weight:700;white-space:nowrap;}
.id-ops {background:#fef3c7;color:#92400e;border:1px solid #fcd34d;border-radius:99px;padding:2px 7px;font-size:9px;font-weight:700;white-space:nowrap;}

/* カスタム赤枠 */
.custom-mark{position:relative;border:2px solid #e53935 !important;border-radius:4px;background:#fff5f5 !important;}
.custom-mark::before{content:"✏️ カスタム";position:absolute;top:-10px;right:4px;background:#fff5f5;color:#c62828;border:1px solid #e53935;font-size:8px;font-weight:700;padding:1px 5px;border-radius:3px;white-space:nowrap;z-index:10;}

/* 画面ラベル・バッジ */
.screen-wrap{
  display:flex;flex-direction:column;align-items:center;
  position:relative; /* ← 必須。省略するとscreen-idsが右上に固まる */
}
.screen-label{font-size:11px;color:#555;margin-top:8px;font-weight:700;}
.screen-sub{font-size:10px;color:#888;margin-top:2px;text-align:center;}
/* screen-ids：phoneフレーム右上オーバーレイ（変更禁止） */
.screen-ids{
  position:absolute;
  top:0;right:-6px;
  display:flex;flex-direction:column;gap:3px;align-items:flex-end;
  z-index:20;
}
.screen-ids span{
  font-size:8px;font-weight:700;
  padding:2px 7px;border-radius:99px;
  box-shadow:0 1px 4px rgba(0,0,0,0.18);
}
```

---

## 確定HTMLテンプレート骨格

```html
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>{キャンペーン名}｜モックアップ</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&display=swap" rel="stylesheet">
<style>/* 上記確定CSSテンプレートをそのまま貼る */</style>
</head>
<body>

<!-- ① タイトルバー -->
<div id="titlebar">
  <div>
    <h1>{キャンペーン名}｜ユーザーフロー モックアップ</h1>
    <p>{プロダクト名} 実UI準拠 ／ 修正箇所のみ赤枠表示</p>
  </div>
  <div style="display:flex;align-items:center;gap:8px;flex-shrink:0;">
    <span class="id-reci">{プロダクト種別バッジ}</span>
    <span class="id-cust">{カスタマイズ種別バッジ}</span>
    <div style="width:1px;height:20px;background:#e0e0e0;margin:0 4px;"></div>
    <div class="legend">
      <div class="legend-item"><div class="lbox c"></div><span>✏️ カスタム変更箇所</span></div>
      <div class="legend-item"><div class="lbox o"></div><span>標準UIそのまま</span></div>
    </div>
  </div>
</div>

<!-- ② フロー図 -->
<div id="flowbar">
  <div class="ftitle">ユーザーフロー図</div>
  <div style="overflow-x:auto;">
    <div style="display:flex;align-items:center;gap:0;min-width:1020px;">
      <!-- フローノード -->
    </div>
  </div>
</div>

<!-- ③ 左右パネル（テーブル固定） -->
<table id="panels-table">
  <tbody><tr>

    <!-- 左2/3：画面フロー -->
    <td id="td-left">
      <div id="td-left-inner">
        <div class="grid">
          <!-- screen-wrap × N画面 -->
        </div>
      </div>
    </td>

    <!-- 右1/3：工数算定 -->
    <td id="td-right">
      <div id="td-right-inner">
        <!-- 工数算定テーブル -->
      </div>
    </td>

  </tr></tbody>
</table>

</body>
</html>
```

---

## SmartStampCard 実UI CSSパターン

```css
.bg-mint{background:#eaf6f2;}
.phone{width:280px;background:#fff;border-radius:36px;border:1.5px solid #ccc;box-shadow:0 4px 16px rgba(0,0,0,0.13);padding:10px;}
.phone-inner{border-radius:28px;overflow:hidden;background:#fff;min-height:560px;display:flex;flex-direction:column;border:1px solid #e8e8e8;}
.sb{display:flex;justify-content:space-between;padding:7px 16px 3px;font-size:9px;font-weight:700;color:#333;background:#fff;}
.stamp{aspect-ratio:1;border-radius:50%;display:flex;flex-direction:column;align-items:center;justify-content:center;}
.stamp.got{background:#4caf50;} .stamp.empty{background:#e0e0e0;}
.s-bar{width:60%;height:2px;background:rgba(255,255,255,0.5);border-radius:1px;margin-bottom:1px;}
.s-num{font-size:11px;font-weight:700;color:#fff;line-height:1;}
.stamp.empty .s-num{color:#bbb;} .stamp.empty .s-bar{background:rgba(0,0,0,0.1);}
.btn-red{background:#c8373a;color:#fff;border:none;border-radius:6px;width:100%;padding:11px;font-size:12px;font-weight:700;font-family:inherit;display:block;}
.btn-outline{background:#fff;color:#555;border:1px solid #ccc;border-radius:6px;width:100%;padding:9px;font-size:11px;font-family:inherit;display:block;margin-top:6px;}
.btn-green{background:#4caf50;color:#fff;border:none;border-radius:6px;width:100%;padding:10px;font-size:12px;font-weight:700;font-family:inherit;display:block;}
.btn-line{background:#06c755;color:#fff;border:none;border-radius:6px;width:100%;padding:12px;font-size:13px;font-weight:700;font-family:inherit;}
.prize-card{background:#fff;border-radius:8px;border:1.5px solid #4caf50;overflow:hidden;}
.prize-pt-bar{background:#4caf50;color:#fff;font-size:10px;font-weight:700;padding:3px 10px;}
```

---

## 工数算定テーブルパターン

```html
<div style="font-size:12px;font-weight:700;color:#333;border-left:4px solid #e87722;padding-left:8px;margin-bottom:6px;">工数算定依頼ポイント</div>
<div style="font-size:10px;color:#888;margin-bottom:12px;">赤枠箇所に対応するカスタム開発要件<br>※標準機能・管理画面設定のみで対応できる箇所は含まない</div>
<div style="overflow-x:auto;">
<table style="width:100%;border-collapse:collapse;font-size:11px;">
  <thead>
    <tr style="background:#e87722;color:#fff;">
      <th style="padding:9px 12px;width:10%;white-space:nowrap;">ID</th>
      <th style="padding:9px 12px;width:10%;">区分</th>
      <th style="padding:9px 12px;width:18%;">機能名</th>
      <th style="padding:9px 12px;width:38%;">要件概要</th>
      <th style="padding:9px 12px;width:10%;text-align:center;">難易度</th>
      <th style="padding:9px 12px;width:14%;">確認事項</th>
    </tr>
  </thead>
  <tbody>
    <tr style="border-bottom:1px solid #f0f0f0;">
      <td style="padding:9px 12px;white-space:nowrap;">
        <span class="id-cust" style="font-size:10px;padding:3px 8px;">F-01</span>
      </td>
      <td><span style="background:#dbeafe;color:#1d4ed8;font-size:9px;font-weight:700;padding:2px 7px;border-radius:3px;">フロント</span></td>
      <td style="font-weight:600;color:#333;">機能名</td>
      <td style="color:#555;line-height:1.6;">要件概要</td>
      <td style="text-align:center;"><span style="background:#fef3c7;color:#92400e;font-size:10px;font-weight:700;padding:3px 10px;border-radius:99px;">小〜中</span></td>
      <td style="font-size:10px;color:#e53935;">確認事項</td>
    </tr>
  </tbody>
</table>
</div>
```

---

## 新プロダクト対応手順

1. PDFをアップロードしてもらう
2. `pdftoppm -jpeg -r 180 -f {page} -l {page} {pdf} /tmp/p{page}` でラスタライズ
3. 実UIの色・ボタン・フォントを確認（**OCR結果はユーザー画面に出ないことに注意**）
4. `references/products.md` に仕様・料金を追記
5. 上記CSSパターンに該当プロダクトのスタイルを追記
6. モックアップ生成時は必ずこのファイルの確定CSSテンプレートを使用すること
---

## ⚠️ レイアウトルール変更プロセス（必須）

このファイルに記載のルールは過去の試行錯誤の末に確定したものです。
変更する前に必ず以下のプロセスを踏むこと：

### 変更確認フロー
```
1. 変更したいルールと理由をユーザーに提示
   「〇〇を□□に変更してよいですか？理由：△△」

2. ユーザーの明示的な承認を待つ（「はい」「OK」等）

3. 承認後のみ変更を実施

4. 変更後はこのファイルの該当箇所も必ず更新
```

### 変更してはいけないルール（絶対禁止）
以下は過去に繰り返し問題を起こした箇所のため、
ユーザーが明示的に指示した場合でも理由を説明して代替案を提示すること：

| 変更禁止 | 理由 |
|---------|------|
| flexbox で左右分割 | iframeで崩れる（何度も確認済み） |
| `height:calc()` でtd-inner固定 | スクロール途中切れの原因（確認済み） |
| td-left を %幅指定 | phone右に大きな空白が出る |
| screen-wrap を省略・ネスト | 後続画面が「下段」にズレる原因 |
| td-inner から height を省略する | overflow-y:auto が機能せずスクロールが消える |
| screen-wrap から position:relative を省略する | screen-ids バッジが画面右上に固まる |
---

## 各画面の正しいHTML構造（変更禁止）

```html
<!-- 1画面 = 必ずこの3階層 -->
<div class="screen-wrap">               <!-- ① wrap開始 -->
  <div class="phone">
    <div class="phone-inner">
      <div class="sb"><span>9:41</span><span>▲◀🔋</span></div>
      <!-- 画面コンテンツ -->
    </div>
  </div>                                <!-- phone閉じ -->
  <div class="screen-label">① 画面名</div>
  <div class="screen-sub">説明テキスト</div>
  <div class="screen-ids">
    <span class="id-cust">F-01</span>
  </div>
</div>                                  <!-- ② wrap閉じ -->

<!-- 次の画面 = 独立したwrapで開始 -->
<div class="screen-wrap">
  ...
</div>
```

**NG例（前画面のwrapが閉じていない）：**
```html
<!-- ❌ これをやると後続画面が下段にズレる -->
<div class="screen-wrap">
  <div class="phone">...</div>
  <div class="screen-label">⑤' 画面名</div>
</div>
                     ← ここでwrapが閉じているが...
<div class="screen-label">⑦ 画面名</div>  ← screen-wrapが欠落！
<div class="screen-ids">...</div>
</div>                ← 前のwrapを閉じてしまう
```
