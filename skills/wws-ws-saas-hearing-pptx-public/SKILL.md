---
name: wws-ws-saas-hearing-pptx-public
description: |
  WWSプロダクト（SmartStampCard・SmartRecieco・SmartStampRally・MapPenguin等）を使った
  提案先（エンドクライアント・代理店）へ見せるA4横パワポ資料を生成するスキル。
  以下のシーンで必ず使用すること：
  - 「提案資料を作って」「パワポを作って」「クライアントに見せる資料」などの依頼が来たとき
  - 社内総合シート（mockup_v*.html）とは別に、外向け提案PPTXが必要なとき
  - コンペ用・営業用のスライドデッキを作りたいとき
---

# 提案先向け ピッチデッキ作成スキル
## 対象：WWSプロダクト全般 ／ A4横 PowerPoint（.pptx）

提案先に直接見せる外向け資料。社内総合シートとは別物。
見た目・わかりやすさ・ストーリーを最優先にする。

---

## 前提

- 実装は `generate_pitch.js`（テンプレートから生成）+ `pptxgenjs` で行う
- テンプレート：`C:\Users\kamat\scripts\_tpl_generate_pitch.js`（新案件では自動コピー済み）
- A4横（LAYOUT_WIDE: 13.33" × 7.5"）固定
- 日本語フォント：`fontFace: 'Meiryo'`（全テキストに必ず指定）
- 実行コマンド：`node generate_pitch.js`（プロジェクトフォルダで実行）

---

## スライド構成（固定）

```
P1.  表紙
P2.  クライアント要件
P3.  提案方針（3段階プラン）
P4.  技術的な合意事項
P5.  カスタム開発箇所
──── P6以降：案件固有スライド ────
P6.  ユーザーフロー図
P7〜. 画面フロー（各ページ3〜4画面）
```

**P1〜P5 は PITCH_CONFIG から自動生成。P6 以降はファイル末尾に手動追加。**

---

## STEP 1｜情報収集

`project_summary_{案件名}.md` と `mockup_v*.html`（社内総合シート）を読み込む。
不足があれば1点だけ質問。

```
【必須】
- キャンペーン名・クライアント名・提案元・日付
- 使用プロダクト・プラン・カスタム開発箇所（F/B番号）
- 課題・要件（P2）・費用感（P3）

【あれば】
- コンペ有無・フェーズ戦略
- ユーザーフロー詳細（P6）
- 画面フロー（P7〜）に載せる主要画面
```

---

## STEP 2｜generate_pitch.js の生成手順

### (1) テンプレートをコピー（new-project.ps1 実行済みなら不要）

```powershell
Copy-Item C:\Users\kamat\scripts\_tpl_generate_pitch.js .\generate_pitch.js
```

### (2) PITCH_CONFIG を案件情報で埋める

`generate_pitch.js` 冒頭の `PITCH_CONFIG` オブジェクトを書き換える。

```javascript
const PITCH_CONFIG = {
  title:     'キャンペーン名',
  titleSub:  'サブタイトル',
  titleAcc:  '使用プロダクト',
  client:    'クライアント名',
  proposer:  'ビザビ株式会社',
  date:      '2026.05',
  products:  ['SmartStampCard'],
  outputFile: 'generated/pitch_{案件名}_{yyyymm}.pptx',

  p2sub: 'P2のサブタイトル',
  reqBlocks: [
    { title: 'GPS判定の仕組み', border: '4CAF50', label: 'バス・電車', desc: '説明...' },
    // 2〜3ブロック
  ],
  reqItems: [
    { icon: '✅', text: '要件A' },
    { icon: '⚡', text: 'コンペ特記事項', emph: true },
  ],

  p3sub: '提案の方針',
  plans: [
    { name: '標準プラン', cost: '費用感：低', features: ['...'], rec: false },
    { name: '推奨プラン', cost: '費用感：中', features: ['...'], rec: true },
    { name: 'MVP',       cost: '費用感：低', features: ['...'], rec: false },
  ],
  phaseStrategy: 'Ph.1 → ...　→　Ph.2 → ...',

  p4sub: '技術合意',
  techBlocks: [
    { title: '仕組みA', color: '4CAF50', rows: [['①', '説明']] },
  ],

  p5sub: '拡張開発対象（F-01〜B-04）',
  customItems: [
    { id: 'F-01', type: 'フロント', name: '機能名', desc: '概要', diff: '小〜中' },
  ],

  totalPages: 9,  // 実際のページ数に合わせる
};
```

### (3) P6以降のスライドを追記

`generate_pitch.js` 末尾のコメントを参考に、ユーザーフロー・画面フロースライドを追加。

**ユーザーフロー（P6）の骨格：**
```javascript
const s6 = pres.addSlide();
addTitleBar(s6, 'ユーザーフロー', 'P2の判定ロジックをフロー図で表現');
pageNum(s6, 6, PITCH_CONFIG.totalPages);
{
  const nW=1.55, nH=0.8;
  // flowNode() / arrowH() / lineH() / lineV() でフロー図を描く
  // 詳細は references/slide-templates.md「P6」セクション参照
}
```

**画面フロー（P7〜）の骨格：**
```javascript
const s7 = pres.addSlide();
addTitleBar(s7, '画面フロー ①　...', '...');
pageNum(s7, 7, PITCH_CONFIG.totalPages);
{
  const {pw, ph, phoneY, xs} = getPhoneLayout(3);  // 2, 3, 4 から選択
  // drawPhone() で各画面を描く
  // badge() でF-xx/B-xxバッジを付ける
  // 詳細は references/slide-templates.md「P7〜9」セクション参照
}
```

---

## STEP 3｜デザイン原則

### カラーパレット（WWSブランド準拠）
| 用途 | カラー |
|------|--------|
| アクセント（オレンジ） | `E87722` |
| サブカラー（グリーン） | `4CAF50` |
| 標準機能バッジ背景 | `DCFCE7` / テキスト `166534` |
| カスタム開発バッジ背景 | `FEE2E2` / テキスト `991B1B` |
| Receicoバッジ背景 | `DBEAFE` / テキスト `1E40AF` |
| 設定作業バッジ背景 | `FEF3C7` / テキスト `92400E` |

### レイアウト原則
- 1スライドにつき1メッセージ
- 箇条書きは1項目1行・最大5項目まで
- 画面フロー：2列レイアウトは `getPhoneLayout(4)` で自動計算

---

## STEP 4｜画面の選定基準（P7〜）

**優先度 高（必ず含める）：**
- カスタムバッジ（F-01〜F-05 / B-01〜B-04）が付いている画面

**優先度 中（スペースがあれば）：**
- フロー上の起点・分岐点（LINEログイン・マイページ）

**優先度 低（省略可）：**
- 標準UIのみ・変更点ゼロの画面

**1ページ = 3〜4画面、最大12画面（3ページ×4画面）**

---

## STEP 5｜QAとファイル出力

```powershell
# プロジェクトフォルダで実行
node generate_pitch.js
# → generated/pitch_{案件名}_{yyyymm}.pptx が出力される
```

**出力後のチェック：**
- [ ] PowerPointで開いて全スライドのフォントが正常か
- [ ] 日本語文字化けがないか（`fontFace:'Meiryo'` 指定漏れ）
- [ ] PITCH_CONFIG の内容が正しく反映されているか
- [ ] ページ番号が `P1 / {totalPages}` から正しく振られているか
- [ ] カスタムバッジの色が正しいか（赤系 = カスタム、黄系 = 設定作業）

### ファイル命名規則
```
generated/pitch_{case_slug}_{yyyymm}.pptx
```

---

## STEP 6｜次フェーズへの引き渡し

作成した PPTX は提案先への直接提出用。
社内向けの技術要件書（工数算定）は別ファイル（`requirements_{案件名}_{yyyymm}.md`）。

---

## 注意事項

- **外向け資料のため**：「カスタムマーク」「SE算定」「B-01」等の社内用語は使わない
- **OCR結果はユーザー画面に出ない**：画面フローに描かない
- **費用は税抜きと明記**
- **カスタム開発の表記**：提案先には「標準機能＋拡張」等の表現に言い換える
- **`fontFace: 'Meiryo'`** は全テキスト要素に必ず指定すること

---

## 参照ファイル

- `references/slide-templates.md` ← P6〜P11 詳細コードパターン（transit実績あり）
- `C:\Users\kamat\scripts\_tpl_generate_pitch.js` ← 生成テンプレート本体
- 社内総合シート `mockup_v*.html` ← コンテンツ転用元
