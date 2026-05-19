---
name: wws-pitch-deck
description: |
  WWSプロダクト（SmartStampCard・SmartRecieco・SmartStampRally・SmartPR全般）を使った
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

- このスキルを使う前に `/mnt/skills/public/pptx/SKILL.md` を必ず読むこと
- 実装は `pptxgenjs.md` に従い `pptxgenjs` で生成
- A4横（LAYOUT_WIDE: 13.33" × 7.5"）固定
- 日本語フォント：Noto Sans JP（フォールバック：Meiryo / メイリオ）
- 生成後は必ず `/mnt/skills/public/pptx/SKILL.md` の Visual QA を実施

---

## スライド構成（固定）

```
┌──────────────────────────────────────────┐
│ 1. 表紙                                   │
│ 2. 課題・背景（クライアントが抱える問題）     │
│ 3. 提案概要（何を、どのプロダクトで解決するか）│
│ 4. システム構成・プロダクト選定              │
│ 5. 費用感・スケジュール                     │
├──────────────────────────────────────────┤
│ 6. ユーザーフロー（1ページ）                │
├──────────────────────────────────────────┤
│ 7〜9. 画面フロー（1〜3ページ、各ページ4画面）│
└──────────────────────────────────────────┘
合計：7〜9ページ
```

---

## STEP 1｜情報収集

社内総合シート（HTML）や機能要件書（MD）が存在する場合は読み込んで内容を転用。
なければ以下をヒアリング（1つだけ質問）：

```
【必須】
- クライアント名・業種・キャンペーン目的
- 使用プロダクト（SmartStampCard / Receico / Rally / SmartPR）
- カスタマイズの有無・主な変更点
- 費用感（概算）・スケジュール

【あれば】
- コンペ有無・競合との差別化ポイント
- 既存の提案方針・スタンス
```

---

## STEP 2｜デザイン方針

### カラーパレット（WWSブランド準拠）
| 用途 | カラー |
|------|--------|
| アクセント（オレンジ） | `E87722` |
| サブカラー（グリーン） | `4CAF50` |
| 背景（白） | `FFFFFF` |
| テキスト（濃グレー） | `333333` |
| ライトグレー | `F5F5F5` |
| 標準バッジ背景 | `DCFCE7` / テキスト `166534` |
| カスタムバッジ背景 | `FEE2E2` / テキスト `991B1B` |

### レイアウト原則
- 1スライドにつき1メッセージ
- 左右2分割（テキスト左・ビジュアル右）or 上下（タイトル＋コンテンツ）
- アイコン・図形を使い、テキスト量を絞る
- 箇条書きは1項目1行・最大5項目まで

---

## STEP 3｜スライド詳細仕様

### スライド1：表紙
```
レイアウト：フルカバー（暗め背景＋白テキスト）
背景色：333333（深グレー）または E87722（オレンジ）
要素：
  - キャンペーン名（36pt・bold・白）
  - クライアント名（16pt・白・60%opacity）
  - プロダクト名バッジ（小さく・右下）
  - 提案元（ビザビ）・日付（右下・小）
```

### スライド2：課題・背景
```
レイアウト：左テキスト・右アイコンor図
要素：
  - タイトル「クライアントが抱える課題」（24pt・bold・E87722）
  - 課題カード3〜4個（アイコン＋見出し＋1行説明）
  - 各カードに薄グレー背景（F5F5F5）・角丸
```

### スライド3：提案概要
```
レイアウト：3列カード（標準 / 推奨 / フルカスタム）
要素：
  - タイトル「ご提案内容」
  - 推奨プランを中央・オレンジ強調（border: E87722）
  - 各カードに：プラン名・費用感・主な機能・納期
  - 「コンペ推奨」バッジ（推奨のみ）
```

### スライド4：システム構成・プロダクト
```
レイアウト：フロー図＋プロダクト説明
要素：
  - ユーザー → プロダクト → 事務局 の流れを横矢印で
  - 各プロダクトボックス（名称・役割・1行説明）
  - カスタム箇所は赤枠 or オレンジ強調
```

### スライド5：費用感・スケジュール
```
レイアウト：左費用テーブル・右スケジュール（ガントバー）
要素：
  - 費用：初期費用・月額・オプション合計
  - スケジュール：要件確認→開発→テスト→リリースの4フェーズ
  - フェーズ提案がある場合はPh.1/Ph.2に分けて表示
```

### スライド6：ユーザーフロー
```
レイアウト：横長フロー図（全幅）
要素（社内シートのフロー図を簡略化して転用）：
  - 左端から右端へ：認証 → アクション → ポイント → 特典
  - 分岐（GPS / レシート）は上下2段
  - 各ノード：プロダクト名色で色分け（標準=緑 / カスタム=赤枠）
  - ノード下に担当システム名（小さく）
```

### スライド7〜9：画面フロー（カスタマイズ含む主要画面）
```
1ページ = 4画面まで
レイアウト：2列×2行（各セル）

各セルの構成：
  ┌─────────────────┐
  │  画面番号・名称   │  ← 上部ラベル（12pt・bold）
  │  [phoneモック]   │  ← 中央にphoneフレーム画像 or 図形
  │  [カスタムバッジ] │  ← 右上にF-01等のバッジ
  │  変更点1行       │  ← 下部に赤テキストで差分説明
  └─────────────────┘

選定基準（カスタマイズ優先）：
  - カスタムバッジ（F-xx / B-xx）がついている画面を優先
  - 標準UIのみの画面は省略可
  - 最大12画面（3ページ×4画面）
```

---

## STEP 4｜pptxgenjs実装

### セットアップ
```bash
npm install -g pptxgenjs
```

### 基本構造テンプレート
```javascript
const pptxgen = require("pptxgenjs");
const pres = new pptxgen();
pres.layout = 'LAYOUT_WIDE';   // 13.33" × 7.5"（A4横相当）
pres.title = '{キャンペーン名}';

// 定数
const C = {
  orange:  'E87722',
  green:   '4CAF50',
  dark:    '333333',
  gray:    'F5F5F5',
  white:   'FFFFFF',
  custBg:  'FEE2E2',
  custTx:  '991B1B',
  stdBg:   'DCFCE7',
  stdTx:   '166534',
  reciBg:  'DBEAFE',
  reciTx:  '1E40AF',
  W: 13.33,  // スライド幅
  H: 7.5,    // スライド高
};

// ── スライド追加ヘルパー ──
function addTitleBar(slide, title, sub) {
  // オレンジの左ボーダー＋タイトル
  slide.addShape(pres.ShapeType.rect, {
    x: 0.4, y: 0.3, w: 0.05, h: 0.6, fill: { color: C.orange }
  });
  slide.addText(title, {
    x: 0.55, y: 0.25, w: 10, h: 0.7,
    fontSize: 22, bold: true, color: C.dark, fontFace: 'Meiryo', margin: 0
  });
  if (sub) slide.addText(sub, {
    x: 0.55, y: 0.9, w: 10, h: 0.35,
    fontSize: 11, color: '888888', fontFace: 'Meiryo', margin: 0
  });
}

function addBadge(slide, label, x, y, type='cust') {
  const bg = type === 'std' ? C.stdBg : type === 'reci' ? C.reciBg : C.custBg;
  const tx = type === 'std' ? C.stdTx : type === 'reci' ? C.reciTx : C.custTx;
  slide.addText(label, {
    x, y, w: 0.7, h: 0.22,
    fontSize: 8, bold: true, color: tx,
    fill: { color: bg }, line: { color: tx, width: 0.5 },
    align: 'center', valign: 'middle',
    fontFace: 'Meiryo', margin: 2
  });
}

function addPhoneFrame(slide, x, y, w, h, screenLabel) {
  // phoneフレーム（角丸長方形）
  slide.addShape(pres.ShapeType.roundRect, {
    x, y, w, h,
    fill: { color: 'FFFFFF' },
    line: { color: 'CCCCCC', width: 1.5 },
    rectRadius: 0.15
  });
  // ステータスバー
  slide.addShape(pres.ShapeType.rect, {
    x: x + 0.05, y: y + 0.05, w: w - 0.1, h: 0.2,
    fill: { color: 'F8F8F8' }, line: { color: 'EEEEEE', width: 0.5 }
  });
  slide.addText('9:41          ▲◀ 🔋', {
    x: x + 0.08, y: y + 0.06, w: w - 0.15, h: 0.15,
    fontSize: 5, color: '333333', fontFace: 'Meiryo', margin: 0
  });
  // 画面ラベル（phoneの中）
  if (screenLabel) {
    slide.addText(screenLabel, {
      x: x + 0.05, y: y + 0.3, w: w - 0.1, h: h - 0.4,
      fontSize: 8, color: '666666', fontFace: 'Meiryo',
      align: 'center', valign: 'middle', margin: 4
    });
  }
}

pres.writeFile({ fileName: '/mnt/user-data/outputs/{filename}.pptx' });
```

### 画面フロースライドのレイアウト（4画面/ページ）
```javascript
// 4画面の配置座標（2列×2行）
const SCREEN_POSITIONS = [
  { x: 0.4,  y: 1.2 },  // 左上
  { x: 6.9,  y: 1.2 },  // 右上
  { x: 0.4,  y: 4.1 },  // 左下
  { x: 6.9,  y: 4.1 },  // 右下
];
const PHONE_W = 2.5;  // phoneフレーム幅
const PHONE_H = 2.7;  // phoneフレーム高

SCREEN_POSITIONS.forEach((pos, i) => {
  if (!screens[i]) return;
  const s = screens[i];
  // phoneフレーム
  addPhoneFrame(slide, pos.x, pos.y + 0.3, PHONE_W, PHONE_H, s.content);
  // 画面名
  slide.addText(s.label, {
    x: pos.x, y: pos.y, w: PHONE_W, h: 0.3,
    fontSize: 10, bold: true, color: C.dark, fontFace: 'Meiryo', margin: 0
  });
  // バッジ
  if (s.badge) addBadge(slide, s.badge, pos.x + PHONE_W - 0.75, pos.y, s.badgeType);
  // 変更点
  if (s.diff) slide.addText(s.diff, {
    x: pos.x, y: pos.y + 0.3 + PHONE_H + 0.05, w: PHONE_W, h: 0.3,
    fontSize: 8, color: C.custTx, fontFace: 'Meiryo', margin: 0
  });
});
```

---

## STEP 5｜画面の選定・グルーピング

社内総合シートの画面フロー（HTML）から以下の優先順位で画面を選ぶ：

**優先度 高（必ず含める）：**
- カスタムバッジ（F-01〜F-05 / B-01〜B-04）がついている画面

**優先度 中（スペースがあれば）：**
- フロー上の起点・分岐点（LINEログイン・マイページ）

**優先度 低（省略可）：**
- 標準UIのみ・変更点ゼロの画面（応募フォーム等）

**1ページ = 4画面。ページ数は1〜3ページ（最大12画面）。**

---

## STEP 6｜QAとファイル出力

1. pptxgenjs でファイル生成
2. LibreOffice + pdftoppm でサムネイル化
3. `/mnt/skills/public/pptx/SKILL.md` の Visual QA を実施
4. 問題があれば修正・再生成
5. `present_files` で提出

### ファイル命名規則
```
pitch_{client_or_project}_{yyyymm}.pptx
```

---

## 注意事項

- **外向け資料のため**：社内用語（「カスタムマーク」「SE算定」等）は使わない
- **OCR結果はユーザー画面に出ない**：画面フローに描かない
- **費用は税抜きと明記**
- **「カスタム」表記**：提案先には「標準機能＋拡張」等の表現に言い換える
- **pptxgenjsのフォント**：`fontFace: 'Meiryo'` を全テキストに指定（日本語文字化け防止）
- **座標単位はインチ**：A4横 = 13.33" × 7.5"

---

## 参照ファイル
- `/mnt/skills/public/pptx/SKILL.md` ← pptx生成の基本ルール（必読）
- `/mnt/skills/public/pptx/pptxgenjs.md` ← pptxgenjs詳細
- `references/slide-templates.md` ← 各スライドの詳細テンプレートコード
- 社内総合シート（`mockup_v*.html`）← コンテンツ転用元
