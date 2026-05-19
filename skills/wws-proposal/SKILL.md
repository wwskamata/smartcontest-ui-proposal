---
name: wws-proposal
description: |
  WWSプロダクト（SmartStampCard・SmartRecieco・SmartStampRally・SmartPR全般）を使った
  キャンペーン提案資料・仕様相談資料を作成するためのスキル。
  依頼者はエンドクライアント・代理店・自社営業を問わず、業種も不問。
  以下のシーンで必ず使用すること：
  - キャンペーンやプロモーションの相談を受けたとき
  - WWSまたはSmartPRプロダクトの提案・比較・選定を検討するとき
  - 画面モックアップ・ユーザーフロー図・工数算定依頼資料を作りたいとき
  - PDFのサービス説明資料を読み込んで提案内容に反映したいとき
  - 「プランを出して」「モックアップ作って」「SE向けに要件まとめて」などの依頼が来たとき
  - 「どのプロダクトが向いているか」という選定相談が来たとき
---

# キャンペーン提案・仕様相談資料作成スキル
## 対象：WWSプロダクト ／ SmartPRシリーズ 全般

依頼者・業種・エンド・代理店を問わず利用可能。
WWSおよびSmartPRシリーズのプロダクトを使ったキャンペーン提案を
素早く・正確に行うためのスキル。

---

## 前提知識（メモリ参照）

このスキルを使う際は必ずユーザーメモリを確認し、以下を把握してから作業する：
- 依頼者の属性（エンド / 代理店 / 自社営業）と業種
- 過去に提案したプロダクト構成・案件名（記録があれば）
- 担当者の役割（営業 / ディレクター / SE など）

---

## ワークフロー

### STEP 1｜相談内容の構造化

相談テキストを受け取ったら、以下の軸で整理する：

```
【依頼者】エンド / 代理店 / 自社　・業種（小売・食品・交通・行政 等）
【目的】何を達成したいか
【対象ユーザー】誰が参加するか・認証方式の制約
【キャンペーン形式】スタンプ・レシート・SNS・クーポン 等
【課題】何が難しいか・現場制約
【制約】技術・運用・予算・納期
【必須要件】絶対に満たすべき機能
【あると良い要件】差別化になる機能
```

整理後、確認事項があれば1つだけ質問してから次のSTEPへ。

---

### STEP 2｜プロダクト選定

`references/products.md` を参照し、要件に最適なプロダクト構成を選定する。

**選定の基本ロジック：**

| 要件 | 候補プロダクト |
|------|--------------|
| GPS位置情報でスタンプ取得 | SmartStampCard / SmartStampRally |
| 複数スポットを順番に巡る | SmartStampRally |
| レシート・領収書で購買証明 | SmartRecieco |
| SNS投稿・UGC連動 | SmartPRシリーズ（詳細はproducts.md参照） |
| ポイント累積・マイページ管理 | SmartStampCard（マイレージOP） |
| クーポン・デジタルギフト配布 | SmartStampCard（OP） |
| 複数プロダクト連携 | カスタマイズプランを前提に提案 |

**プラン提示の型（3段階）：**
1. **標準プラン**：既存機能のみ・最短納期・低コスト
2. **推奨プラン**：一部カスタマイズ・バランス型（コンペ推奨）
3. **フルカスタムプラン**：完全要件対応・高コスト

---

### STEP 3｜PDF資料の読み込み（アップロード時）

プロダクトのPDF説明資料がアップロードされた場合：

```bash
pdftoppm -jpeg -r 180 -f {page} -l {page} {pdf_path} /tmp/page_p{page}
```

確認すべきページ：参加者フロー画面・機能一覧・料金表

⚠️ PDF画像で確認した実UIと異なる機能をモックアップに描かない。
新プロダクトを読み込んだ場合は `references/products.md` に仕様・料金を追記すること。

---

### STEP 4｜ユーザーフロー設計

フロー設計前に確認すること：
- 認証方式（LINEログイン / メールアドレス / SNS）
- 付与タイミング（即時 / 審査後）
- 特典形式（後日抽選 / インスタントウィン / クーポン）
- 事務局オペレーション頻度（日次 / リアルタイム / 無人）

---

### STEP 5｜モックアップ生成

`references/mockup-layout.md` を必ず参照してからHTMLを生成すること。

**構成（固定ルール）：**
```
① タイトルバー（固定・1カラム）
② ユーザーフロー図（固定・1カラム・横スクロール対応）
③ 左右分割エリア（独立スクロール）
   ├── 左（width:320px固定）：画面フロー（縦1列・中央揃え）
   └── 右（残り幅）：工数算定依頼テーブル
```

**絶対に守るルール：**
- `display:flex` で左右分割しない → `table-layout:fixed` のテーブルのみ
- `td-left-inner` / `td-right-inner` に `height:calc()` を必ず設定
- `td-left` は `width:320px` 固定（%指定禁止）
- `screen-wrap` に `position:relative` を必ず付ける
- 各画面は独立した `<div class="screen-wrap">` で囲む

詳細CSS・HTML骨格 → `references/mockup-layout.md`

---

### STEP 6｜工数算定依頼資料の生成

`references/se-requirements.md` を参照。

テーブル列：ID / 区分 / 機能名 / 要件概要 / 難易度 / 確認事項

IDバッジ：F-xx/B-xx → 赤（id-cust）、設定作業のみ → 黄（id-ops）

---

### STEP 7｜資料化

```
mockup_v{n}_{project}_{yyyymm}.html
requirements_{project}_{yyyymm}.md
```

---

### STEP 8｜プレビュー検証（必須・最終ステップ）

HTMLファイルを `present_files` で出力した後、必ず以下のPythonチェックを実行すること。
**1項目でも❌があれば自動修正してから再提出する。ユーザーへの提出は全✅になってから。**

```python
with open('{出力ファイルパス}', 'r') as f:
    html = f.read()
import re

checks = {
    'table-layout:fixed'            : 'table-layout:fixed' in html,
    'td-left 320px固定'             : 'width:320px' in html,
    'td-inner height:calc あり'     : html.count('height:calc') >= 2,
    'td-inner overflow-y:auto あり' : html.count('overflow-y:auto') >= 2,
    'grid flex-direction:column'    : 'flex-direction:column' in html,
    'grid align-items:center'       : 'align-items:center' in html,
    'screen-wrap position:relative' : bool(__import__('re').search(r'\.screen-wrap\{([^}]*)\}', html) and 'position:relative' in __import__('re').search(r'\.screen-wrap\{([^}]*)\}', html).group(1)),
    'screen-ids position:absolute'  : bool(__import__('re').search(r'\.screen-ids\{[^}]*position:absolute', html, __import__('re').DOTALL)),
    'custom-mark 白文字なし'         : 'color:#c62828' in html,
    '全画面存在（screen-label数≥1）': len(re.findall(r'<div class="screen-label">', html)) >= 1,
    'screen-wrap数 = screen-label数': (
        len([m for m in re.finditer(r'<div class="screen-wrap">', html)
             if 'display' not in html[m.start():m.start()+50]])
        == len(re.findall(r'<div class="screen-label">', html))
    ),
    '画面フローがtd-left内'          : html.index('id="td-right"') > html.index('id="td-left"'),
    '工数テーブルがtd-right内'       : 'F-0' in html[html.index('id="td-right"'):],
}

all_ok = True
for k, v in checks.items():
    print(f"{'✅' if v else '❌'} {k}")
    if not v: all_ok = False
print("\n" + ("✅ 全チェック通過！提出可能" if all_ok else "❌ 修正が必要"))
```

**自動修正の優先順位：**
1. レイアウト崩れ → `references/mockup-layout.md` の確定CSSで上書き
2. スクロールが消えた → `height:calc(100vh - 44px - 116px)` を td-inner に追加
3. バッジが右上に固まる → `.screen-wrap` に `position:relative` を追加
4. 画面欠落・ネスト混入 → 欠落 screen-wrap を再挿入、余分ブロックを切除
5. カスタムラベル白文字 → `.custom-mark::before` の `color` を `#c62828` に修正

修正後は必ずチェックを再実行し、全✅を確認してから `present_files` で再提出する。

---

## 注意事項

- **標準機能と非標準機能を明確に区別する**：「できる」と言う前にPDFで確認
- **OCR結果はユーザー画面に出ない**：管理側の処理のみ
- **ルール変更は確認必須**：`references/mockup-layout.md` のルールを変更する際は必ずユーザーに確認を取ること
- **新プロダクトが登場したら `references/products.md` に追記**

詳細な製品仕様・料金 → `references/products.md`
レイアウトルール・CSS・HTML骨格 → `references/mockup-layout.md`
工数算定テンプレート → `references/se-requirements.md`

---

## 📄 仕様相談シート一式の生成フロー

**このシートとは：**
案件サマリー・ユーザーフロー図・画面カスタマイズモックアップ・工数算定依頼テーブルを
1つのHTMLファイルにまとめた、提案・SE依頼・社内共有用の三点セットシート。

---

### シートの構成（固定）

```
┌─────────────────────────────────────────────────────┐
│ ① タイトルバー（固定）                                 │
│   プロジェクト名 ／ プロダクト種別バッジ ／ 凡例           │
│   「📋 案件サマリー ▼ 開く」ボタン                      │
├─────────────────────────────────────────────────────┤
│ ② 案件サマリー（デフォルト閉じ・クリックで開閉）           │
│   背景色：薄オレンジ（#fff3e0）                          │
│   ボーダー：上下オレンジ（#e87722）                       │
│   「▼ クリックして開く」ヒント（点滅アニメ付き）           │
│   「▲ 閉じる」ボタンで閉じられることを明示               │
│   ── 中身（閉じた状態では非表示） ──                     │
│   1.背景 2.要件 3.提案方針 4.技術合意 5.カスタム開発箇所  │
│   ※ 6.成果物一覧は非表示（内部資料のため）               │
├─────────────────────────────────────────────────────┤
│ ③ ユーザーフロー図（固定・横スクロール対応）              │
├──────────────────┬──────────────────────────────────┤
│ ④-1 工数算定依頼  │ ④-2 画面カスタマイズモックアップ    │
│ （右1/3・スクロール）│ （左2/3・縦1列・独立スクロール）   │
└──────────────────┴──────────────────────────────────┘
```

---

### 生成手順（STEP1〜8に加えて実施）

#### A. 案件サマリーHTMLの生成

以下の6章構成で `project_summary_{project}.html` を生成する（ただし6章は非表示）：

```
1. 案件の背景（依頼者・課題・対象）
2. クライアントの要件（判定ロジック・その他要件）
3. 提案方針（プロダクト選定・3段階プラン・コンペ戦略）
4. 技術的な合意事項（主要フローの設計）
5. カスタム開発が必要な箇所（F/B一覧・WWS確認事項）
6. 成果物一覧 → display:none で非表示
```

#### B. モックアップHTMLへのサマリー埋め込み

以下の仕様でサマリーをアコーディオンとして組み込む：

```css
/* サマリーバー（デフォルト閉じ・強調必須） */
#summary-bar {
  background: #fff3e0;
  border-top: 2px solid #e87722;
  border-bottom: 2px solid #e87722;
}

/* デフォルト閉じ */
#summary-body { display: none; }
#summary-body.open { display: block; max-height: 60vh; overflow-y: auto; }

/* 点滅ヒント（クリックできることを強調） */
.summary-hint {
  border: 1px solid #e87722; border-radius: 99px;
  padding: 1px 8px; color: #e87722; font-size: 9px;
  animation: pulse 2s infinite;
}
@keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:.5;} }

/* 開閉ボタン */
#summary-toggle-arrow {
  background: #e87722; color: #fff;
  padding: 4px 10px; border-radius: 4px;
}
```

```html
<!-- サマリーバーのHTML構造 -->
<div id="summary-bar">
  <button id="summary-toggle" onclick="toggleSummary()">
    <div id="summary-toggle-left">
      <div id="summary-toggle-label">
        <span class="stag">📋 案件サマリー</span>
        {案件名}｜{依頼者} ／ {提案ステータス}
      </div>
      <span class="summary-hint">▼ クリックして開く</span>
    </div>
    <span id="summary-toggle-arrow">▼ 開く</span>
  </button>
  <div id="summary-body">
    {サマリー本文（1〜5章）}
  </div>
</div>

<script>
function toggleSummary(){
  var body  = document.getElementById('summary-body');
  var arrow = document.getElementById('summary-toggle-arrow');
  var hint  = document.querySelector('.summary-hint');
  var isOpen = body.classList.contains('open');
  if(isOpen){
    body.classList.remove('open'); arrow.classList.remove('open');
    arrow.textContent = '▼ 開く';
    if(hint) hint.textContent = '▼ クリックして開く';
  } else {
    body.classList.add('open'); arrow.classList.add('open');
    arrow.textContent = '▲ 閉じる';
    if(hint) hint.textContent = '▲ クリックして閉じる';
  }
}
</script>
```

#### C. STEP8チェックリストへの追加項目

シート一式生成後は以下もチェックに追加：

```python
'サマリー強調UI'     : 'pulse' in html and '▲ 閉じる' in html,
'サマリーデフォルト閉じ': '#summary-body{' in html and 'display:none' in html,
'成果物一覧非表示'   : 'display:none' in html[html.index('成果物一覧'):html.index('成果物一覧')+100],
```

---

### 成果物ファイル命名規則

```
mockup_v{n}_{project}_{yyyymm}.html   ← サマリー埋め込み済みシート一式
requirements_{project}_{yyyymm}.md    ← SE向け機能要件定義書（別ファイル）
```

---

## 🌐 多言語切替（日英）

モックアップHTMLには必ず日英切替ボタンを実装すること。

### 実装方法

**① 翻訳マップをJSオブジェクトとして定義**
```javascript
var TRANS = {
  "日本語テキスト": "English text",
  // ... 全画面のテキストを網羅
};
var currentLang = 'ja';

function toggleLang() {
  currentLang = currentLang === 'ja' ? 'en' : 'ja';
  var btn = document.getElementById('lang-toggle');
  btn.textContent = currentLang === 'ja' ? '🌐 English' : '🌐 日本語';
  btn.style.background = currentLang === 'ja' ? '#555' : '#1d4ed8';
  document.querySelectorAll('[data-i18n]').forEach(function(el) {
    var key = el.getAttribute('data-i18n');
    if (TRANS[key]) {
      el.textContent = currentLang === 'en' ? TRANS[key] : key;
    }
  });
}
```

**② 翻訳対象テキストを `data-i18n` spanでラップ**
```html
<span data-i18n="日本語テキスト">日本語テキスト</span>
```

Pythonで一括変換：
```python
html = re.sub(
    r'(?<=>)(' + re.escape(ja_text) + r')(?=<)',
    f'<span data-i18n="{ja_text}">\1</span>',
    html
)
```

**③ タイトルバーに切替ボタンを追加**
```html
<button id="lang-toggle" onclick="toggleLang()"
  style="background:#555;color:#fff;border:none;border-radius:5px;
         padding:6px 12px;font-size:10px;font-weight:700;cursor:pointer;">
  🌐 English
</button>
```

### STEP8チェックに追加
```python
'言語切替ボタン'    : 'lang-toggle' in html,
'data-i18n 100件以上': html.count('data-i18n=') >= 100,
'翻訳マップ(TRANS)' : 'var TRANS' in html,
```

### 翻訳対象（全エリア必須）
1. **各画面フロー**：ボタン・ラベル・説明文・screen-label/sub
2. **案件サマリー**：章タイトル・テーブル見出し・本文
3. **ユーザーフロー図**：ノードラベル・凡例・ID
4. **工数算定テーブル**：ID・区分・機能名・要件概要・難易度・確認事項

翻訳マップは長いテキストから順にソートして適用すること（短い単語の誤爆を防ぐ）：
```python
sorted_keys = sorted(translations.keys(), key=lambda x: -len(x))
```

STEP8チェックは `data-i18n=` の件数で網羅性を確認（目安：全エリア対応で200件以上）
