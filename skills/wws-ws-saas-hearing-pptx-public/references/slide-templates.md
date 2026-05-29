# スライドテンプレート詳細コード

A4横（13.33" × 7.5"）pptxgenjsで動作確認済みのコードパターン集。

---

## スライド1：表紙

```javascript
const s1 = pres.addSlide();

// 背景（濃グレー）
s1.addShape(pres.ShapeType.rect, {
  x: 0, y: 0, w: C.W, h: C.H, fill: { color: '222222' }
});

// 左端オレンジアクセントライン
s1.addShape(pres.ShapeType.rect, {
  x: 0, y: 0, w: 0.25, h: C.H, fill: { color: C.orange }
});

// キャンペーン名（大タイトル）
s1.addText(campaignName, {
  x: 0.6, y: 2.2, w: 8.5, h: 1.2,
  fontSize: 36, bold: true, color: C.white,
  fontFace: 'Meiryo', margin: 0
});

// クライアント名
s1.addText(clientName, {
  x: 0.6, y: 3.5, w: 8, h: 0.5,
  fontSize: 18, color: 'BBBBBB', fontFace: 'Meiryo', margin: 0
});

// プロダクトバッジ群（右下）
const products = ['SmartStampCard', 'SmartRecieco'];
products.forEach((p, i) => {
  s1.addText(p, {
    x: 9.5, y: 5.8 + i * 0.35, w: 3.2, h: 0.28,
    fontSize: 9, color: C.white, bold: true,
    fill: { color: '444444' }, align: 'center', fontFace: 'Meiryo', margin: 2
  });
});

// 提案元・日付
s1.addText(`${proposerName}　${date}`, {
  x: 0.6, y: 6.9, w: 6, h: 0.3,
  fontSize: 10, color: '888888', fontFace: 'Meiryo', margin: 0
});
```

---

## スライド2：課題・背景

```javascript
const s2 = pres.addSlide();
addTitleBar(s2, 'クライアントが抱える課題', '解決すべき3つのポイント');

// 課題カード（横3列）
const challenges = [
  { icon: '⚠️', title: '交通事業者の負担ゼロ', desc: 'QRコード設置・シール掲出は一切不可' },
  { icon: '🛡️', title: '不正参加の防止',     desc: '車での移動による不正チェックイン排除' },
  { icon: '📱', title: 'アプリ不要',          desc: 'LINEログインのブラウザ参加のみ' },
];

const cardW = 3.8, cardH = 3.5, cardY = 1.8;
const startX = (C.W - cardW * 3 - 0.3 * 2) / 2;

challenges.forEach((ch, i) => {
  const cx = startX + i * (cardW + 0.3);

  // カード背景
  s2.addShape(pres.ShapeType.roundRect, {
    x: cx, y: cardY, w: cardW, h: cardH,
    fill: { color: 'F9F9F9' }, line: { color: 'E0E0E0', width: 0.8 },
    rectRadius: 0.1
  });

  // アイコン
  s2.addText(ch.icon, {
    x: cx, y: cardY + 0.4, w: cardW, h: 0.8,
    fontSize: 32, align: 'center', fontFace: 'Meiryo', margin: 0
  });

  // タイトル
  s2.addText(ch.title, {
    x: cx + 0.15, y: cardY + 1.35, w: cardW - 0.3, h: 0.5,
    fontSize: 13, bold: true, color: C.dark,
    align: 'center', fontFace: 'Meiryo', margin: 0
  });

  // 説明
  s2.addText(ch.desc, {
    x: cx + 0.15, y: cardY + 1.95, w: cardW - 0.3, h: 1.2,
    fontSize: 11, color: '666666', align: 'center',
    fontFace: 'Meiryo', margin: 0, wrap: true
  });
});
```

---

## スライド3：提案概要（3段階プラン）

```javascript
const s3 = pres.addSlide();
addTitleBar(s3, 'ご提案内容', 'ご要件・ご予算に合わせて3プランをご用意');

const plans = [
  { name: '標準プラン',    cost: '初期25万＋月3万〜', features: ['既存機能のみ', '手動スポット登録', '最短10日納期'], recommended: false },
  { name: '推奨プラン',    cost: '初期50〜80万（目安）', features: ['GPS乗降ペア判定', 'Receico連携', 'SSO対応'], recommended: true  },
  { name: 'フルカスタム',  cost: '要相談',              features: ['全要件対応', 'GTFS連携', '完全独自設計'], recommended: false },
];

const pW = 3.6, pH = 5.2, pY = 1.5;
const pStartX = (C.W - pW * 3 - 0.4 * 2) / 2;

plans.forEach((plan, i) => {
  const px = pStartX + i * (pW + 0.4);
  const isRec = plan.recommended;

  // カード背景
  s3.addShape(pres.ShapeType.roundRect, {
    x: px, y: pY, w: pW, h: pH,
    fill: { color: isRec ? 'FFF8F0' : 'FFFFFF' },
    line: { color: isRec ? C.orange : 'E0E0E0', width: isRec ? 2 : 0.8 },
    rectRadius: 0.12
  });

  // 「コンペ推奨」バッジ
  if (isRec) {
    s3.addText('コンペ推奨', {
      x: px + 0.9, y: pY - 0.2, w: 1.8, h: 0.3,
      fontSize: 9, bold: true, color: C.white,
      fill: { color: C.orange }, align: 'center', fontFace: 'Meiryo', margin: 2
    });
  }

  // プラン名
  s3.addText(plan.name, {
    x: px + 0.1, y: pY + 0.3, w: pW - 0.2, h: 0.5,
    fontSize: 15, bold: true, color: isRec ? C.orange : C.dark,
    align: 'center', fontFace: 'Meiryo', margin: 0
  });

  // 費用
  s3.addText(plan.cost, {
    x: px + 0.1, y: pY + 0.9, w: pW - 0.2, h: 0.4,
    fontSize: 11, color: '888888', align: 'center', fontFace: 'Meiryo', margin: 0
  });

  // 区切り線
  s3.addShape(pres.ShapeType.line, {
    x: px + 0.3, y: pY + 1.4, w: pW - 0.6, h: 0,
    line: { color: 'E0E0E0', width: 0.5 }
  });

  // 機能リスト
  plan.features.forEach((f, j) => {
    s3.addText([
      { text: '✓ ', options: { bold: true, color: C.green } },
      { text: f,    options: { color: C.dark } }
    ], {
      x: px + 0.2, y: pY + 1.6 + j * 0.55, w: pW - 0.4, h: 0.45,
      fontSize: 11, fontFace: 'Meiryo', margin: 0
    });
  });
});
```

---

## スライド4：システム構成

```javascript
const s4 = pres.addSlide();
addTitleBar(s4, 'システム構成', '標準機能 ＋ 拡張対応');

// フロー：ユーザー → StampCard → Receico → 事務局
const nodes = [
  { label: 'ユーザー',          sub: 'LINEログイン\nブラウザ参加', color: '4CAF50', tx: 'FFFFFF' },
  { label: 'SmartStampCard',   sub: 'GPS乗降判定\nポイント管理', color: 'E87722', tx: 'FFFFFF' },
  { label: 'SmartRecieco',     sub: 'レシートOCR\nタクシー判定', color: '1D4ED8', tx: 'FFFFFF' },
  { label: '事務局',            sub: '日次審査\n承認・管理',    color: '555555', tx: 'FFFFFF' },
];

const nW = 2.4, nH = 1.4;
const nY = 3.0;
const gap = (C.W - 0.8 - nW * 4) / 3;
const startX = 0.4;

nodes.forEach((node, i) => {
  const nx = startX + i * (nW + gap);

  // ノードボックス
  s4.addShape(pres.ShapeType.roundRect, {
    x: nx, y: nY, w: nW, h: nH,
    fill: { color: node.color }, line: { color: node.color, width: 0 },
    rectRadius: 0.1
  });
  s4.addText(node.label, {
    x: nx, y: nY + 0.15, w: nW, h: 0.4,
    fontSize: 13, bold: true, color: node.tx,
    align: 'center', fontFace: 'Meiryo', margin: 0
  });
  s4.addText(node.sub, {
    x: nx, y: nY + 0.6, w: nW, h: 0.7,
    fontSize: 10, color: node.tx,
    align: 'center', fontFace: 'Meiryo', margin: 0, wrap: true
  });

  // 矢印
  if (i < nodes.length - 1) {
    const ax = nx + nW + 0.05;
    s4.addShape(pres.ShapeType.line, {
      x: ax, y: nY + nH / 2, w: gap - 0.1, h: 0,
      line: { color: 'BBBBBB', width: 1.5, endArrowType: 'open' }
    });
  }
});

// カスタム対応ラベル（StampCard下）
s4.addText('● 乗降ペア判定（拡張）', {
  x: startX + (nW + gap), y: nY + nH + 0.15, w: nW, h: 0.3,
  fontSize: 9, color: C.custTx, align: 'center', fontFace: 'Meiryo', margin: 0
});
s4.addText('● SSO連携（拡張）', {
  x: startX + (nW + gap) * 2, y: nY + nH + 0.15, w: nW, h: 0.3,
  fontSize: 9, color: C.custTx, align: 'center', fontFace: 'Meiryo', margin: 0
});
```

---

## スライド5：費用感・スケジュール

```javascript
const s5 = pres.addSlide();
addTitleBar(s5, '費用感・スケジュール（目安）', '※ 表示価格は税抜き。詳細はお見積り提示');

// 左：費用テーブル
const costItems = [
  ['SmartStampCard 初期費用', '250,000円〜'],
  ['SmartRecieco 初期費用',   '400,000円〜'],
  ['LINEログイン OP',         '+50,000円'],
  ['カスタム開発費',           '別途見積'],
  ['月額費用（合計）',         '50,000円〜/月'],
];

costItems.forEach(([label, val], i) => {
  const ry = 1.6 + i * 0.65;
  const bg = i % 2 === 0 ? 'FAFAFA' : 'FFFFFF';
  s5.addShape(pres.ShapeType.rect, {
    x: 0.4, y: ry, w: 5.5, h: 0.6, fill: { color: bg },
    line: { color: 'EEEEEE', width: 0.5 }
  });
  s5.addText(label, {
    x: 0.5, y: ry + 0.1, w: 3.5, h: 0.4,
    fontSize: 11, color: C.dark, fontFace: 'Meiryo', margin: 0
  });
  s5.addText(val, {
    x: 3.8, y: ry + 0.1, w: 2, h: 0.4,
    fontSize: 11, bold: true, color: C.orange, align: 'right', fontFace: 'Meiryo', margin: 0
  });
});

// 右：スケジュール（ガントバー）
const phases = [
  { name: '要件確認・設計', w: 2.5, color: 'E87722' },
  { name: '開発',          w: 3.0, color: '4CAF50' },
  { name: 'テスト',        w: 1.5, color: '1D4ED8' },
  { name: 'リリース',      w: 0.8, color: '555555' },
];
let gx = 7.2;
phases.forEach((ph, i) => {
  const gy = 2.0 + i * 1.0;
  s5.addText(ph.name, {
    x: 7.2, y: gy, w: 2.2, h: 0.4,
    fontSize: 11, color: C.dark, fontFace: 'Meiryo', margin: 0
  });
  s5.addShape(pres.ShapeType.roundRect, {
    x: 9.5, y: gy + 0.05, w: ph.w, h: 0.35,
    fill: { color: ph.color }, line: { color: ph.color, width: 0 },
    rectRadius: 0.05
  });
  s5.addText(`Week ${i * 2 + 1}〜${i * 2 + Math.round(ph.w)}`, {
    x: 9.5 + ph.w + 0.1, y: gy + 0.05, w: 1.5, h: 0.35,
    fontSize: 9, color: '888888', fontFace: 'Meiryo', margin: 0
  });
});
```

---

## スライド6：ユーザーフロー

```javascript
const s6 = pres.addSlide();
addTitleBar(s6, 'ユーザーフロー', 'GPS乗降判定 ＋ タクシーレシート判定');

// フローノード（横並び・2段）
const flowNodes = [
  // 上段（GPS）
  [
    { label: 'LINEログイン', id: '標準',        color: '4CAF50', tx: 'FFFFFF', row: 0 },
    { label: 'マイページ',   id: 'F-01',        color: 'FEE2E2', tx: '991B1B', row: 0 },
    { label: '乗車GPS',      id: 'F-02/B-01',  color: 'FEE2E2', tx: '991B1B', row: 0 },
    { label: '降車GPS',      id: 'F-02/B-01',  color: 'FEE2E2', tx: '991B1B', row: 0 },
    { label: 'ポイント付与', id: 'B-01',        color: 'FEE2E2', tx: '991B1B', row: 0 },
  ],
  // 下段（タクシー）
  [
    { label: 'レシート登録', id: 'Receico',     color: 'DBEAFE', tx: '1E40AF', row: 1 },
    { label: '事務局審査',   id: 'B-04設定',    color: 'FEF3C7', tx: '92400E', row: 1 },
    { label: 'ポイント連携', id: 'B-02',        color: 'FEE2E2', tx: '991B1B', row: 1 },
  ]
];

const fnW = 1.8, fnH = 0.8, fnGap = 0.3;
const row0Y = 2.2, row1Y = 4.5;

flowNodes[0].forEach((node, i) => {
  const nx = 0.4 + i * (fnW + fnGap);
  s6.addShape(pres.ShapeType.roundRect, {
    x: nx, y: row0Y, w: fnW, h: fnH,
    fill: { color: node.color }, line: { color: node.color, width: 0 },
    rectRadius: 0.08
  });
  s6.addText(node.label, {
    x: nx, y: row0Y + 0.1, w: fnW, h: 0.4,
    fontSize: 11, bold: true, color: node.tx, align: 'center', fontFace: 'Meiryo', margin: 0
  });
  s6.addText(node.id, {
    x: nx, y: row0Y + 0.5, w: fnW, h: 0.25,
    fontSize: 8, color: node.tx, align: 'center', fontFace: 'Meiryo', margin: 0
  });
  if (i < flowNodes[0].length - 1) {
    s6.addShape(pres.ShapeType.line, {
      x: nx + fnW + 0.02, y: row0Y + fnH / 2, w: fnGap - 0.04, h: 0,
      line: { color: 'BBBBBB', width: 1.2, endArrowType: 'open' }
    });
  }
});

// 下段ノード（マイページから分岐）
const mx = 0.4 + (fnW + fnGap);
s6.addShape(pres.ShapeType.line, {
  x: mx + fnW / 2, y: row0Y + fnH, w: 0, h: row1Y - row0Y - fnH,
  line: { color: 'BBBBBB', width: 1, dashType: 'dash' }
});

flowNodes[1].forEach((node, i) => {
  const nx = mx + i * (fnW + fnGap);
  s6.addShape(pres.ShapeType.roundRect, {
    x: nx, y: row1Y, w: fnW, h: fnH,
    fill: { color: node.color }, line: { color: node.color, width: 0 }, rectRadius: 0.08
  });
  s6.addText(node.label, {
    x: nx, y: row1Y + 0.1, w: fnW, h: 0.4,
    fontSize: 11, bold: true, color: node.tx, align: 'center', fontFace: 'Meiryo', margin: 0
  });
  s6.addText(node.id, {
    x: nx, y: row1Y + 0.5, w: fnW, h: 0.25,
    fontSize: 8, color: node.tx, align: 'center', fontFace: 'Meiryo', margin: 0
  });
  if (i < flowNodes[1].length - 1) {
    s6.addShape(pres.ShapeType.line, {
      x: nx + fnW + 0.02, y: row1Y + fnH / 2, w: fnGap - 0.04, h: 0,
      line: { color: 'BBBBBB', width: 1.2, endArrowType: 'open' }
    });
  }
});
```

---

## 凡例ヘルパー（全スライド共通・右下に配置）

```javascript
function addLegend(slide) {
  const items = [
    { color: '4CAF50', label: '標準機能' },
    { color: 'FEE2E2', label: 'カスタム拡張', tx: '991B1B' },
    { color: 'DBEAFE', label: 'Receico',       tx: '1E40AF' },
    { color: 'FEF3C7', label: '設定作業のみ',   tx: '92400E' },
  ];
  items.forEach((item, i) => {
    slide.addShape(pres.ShapeType.rect, {
      x: 10.1 + Math.floor(i / 2) * 1.5, y: 6.6 + (i % 2) * 0.35,
      w: 0.18, h: 0.18,
      fill: { color: item.color }, line: { color: item.tx || item.color, width: 0.5 }
    });
    slide.addText(item.label, {
      x: 10.32 + Math.floor(i / 2) * 1.5, y: 6.6 + (i % 2) * 0.35,
      w: 1.2, h: 0.2,
      fontSize: 8, color: '666666', fontFace: 'Meiryo', margin: 0
    });
  });
}
```
