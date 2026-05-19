# WWS提案プロジェクト｜Claude Code 設定ファイル

このファイルはClaude Codeがプロジェクトフォルダを開いた際に自動で読み込まれます。

---

## プロジェクト概要

**案件**：公共交通利用促進キャンペーン（岡山県想定・行政向けコンペ）  
**依頼者**：ビザビ社（代理店）→ 行政クライアントへ提案  
**プロダクト**：SmartStampCard × SmartRecieco（WWS Corporation）

---

## フォルダ構成

```
wws-local-project/
├── CLAUDE.md                        ← このファイル（常に参照）
├── assets/                          ← 案件成果物
│   ├── mockup_v5_transit_campaign.html   ← 社内総合シート（確定版）
│   ├── mockup_stamp_flow.html            ← 乗車地検索フロー 全13画面
│   ├── project_summary_transit_campaign.html  ← 案件サマリー
│   ├── project_summary_transit_campaign.md    ← 案件サマリー（Markdown）
│   └── custom_requirements_for_SE.md          ← SE向け機能要件定義書
├── generated/                       ← 生成済みPPTX
│   ├── pitch_transit_v3_202505.pptx  ← 最新提案デッキ（推奨）
│   └── pitch_transit_v2_202505.pptx  ← 旧版
└── skills/                          ← スキルファイル
    ├── wws-proposal/                 ← 社内総合シート生成スキル
    │   ├── SKILL.md
    │   └── references/
    │       ├── mockup-layout.md      ← レイアウト確定ルール（必読）
    │       ├── products.md           ← プロダクト仕様・料金
    │       └── se-requirements.md   ← 工数算定テンプレ・スキル名定義
    └── wws-pitch-deck/              ← 提案先向けPPTX生成スキル
        ├── SKILL.md
        └── references/
            └── slide-templates.md   ← 各スライドのpptxgenjsコード
```

---

## 重要な確定仕様（必ず守ること）

### クライアント要件
- 対象交通：バス・電車・路面電車・フェリー（GPS判定）／タクシー（レシートOCR判定）
- 制約：現場負担ゼロ（QR設置不可）・不正防止・アプリDL不要・LINEログインブラウザ参加
- GPS判定：乗車地＋降車地の2点チェックインでペア成立→1pt付与
- タクシー判定：領収書撮影→OCR→事務局日次審査→承認後ポイント付与
- **OCR結果はユーザー画面に表示しない**（管理側のみ）

### スキル名定義（B-01）
| スキル名 | 英名 | 区分 |
|---------|------|------|
| 乗車地検索フロー | `BoardingSearchFlow` | F-02/B-01 |
| 乗降ペア判定 | `BoardingPairJudge` | B-01 |
| 乗車中セッション | `RidingSession` | B-01 |

### プロダクト構成（確定）
| プロダクト | 用途 | プラン |
|-----------|------|--------|
| SmartStampCard | GPS乗降判定・マイページ・ポイント管理・賞品応募 | スタンダード（初期25万・月3万）＋LINEログインOP＋マイレージOP |
| SmartRecieco | タクシー領収書OCR審査・ポイント付与 | WEB版マイレージ＋LINEログインOP |

---

## HTMLモックアップのルール

**詳細は `skills/wws-proposal/references/mockup-layout.md` を参照。**

### 絶対に守ること
- 左右並列は `table-layout:fixed` のHTMLテーブルのみ（flexbox禁止）
- `td-left-inner` / `td-right-inner` に `height:calc(100vh - 44px - 116px)` を必ず設定
- `td-left` は `width:320px` 固定（%指定禁止）
- `screen-wrap` に `position:relative` を必ず付ける
- 各画面は独立した `<div class="screen-wrap">` で囲む

### レイアウト構成（固定）
```
左（320px固定）：画面フロー（縦1列・中央揃え）
右（残り幅）  ：工数算定依頼テーブル
```

### STEP8チェック（生成後に必ず実行）
```python
with open('{path}', 'r') as f: html = f.read()
import re
checks = {
    'table-layout:fixed'            : 'table-layout:fixed' in html,
    'td-left 320px固定'             : 'width:320px' in html,
    'td-inner height:calc あり'     : html.count('height:calc') >= 2,
    'td-inner overflow-y:auto あり' : html.count('overflow-y:auto') >= 2,
    'screen-wrap position:relative' : bool(re.search(r'\.screen-wrap\{([^}]*)\}', html) and
                                      'position:relative' in re.search(r'\.screen-wrap\{([^}]*)\}', html).group(1)),
    'screen-ids position:absolute'  : bool(re.search(r'\.screen-ids\{[^}]*position:absolute', html, re.DOTALL)),
    'custom-mark 白文字なし'         : 'color:#c62828' in html,
    '全画面存在'                     : len(re.findall(r'<div class="screen-label">', html)) >= 1,
    'screen-wrap数 = screen-label数': (
        len([m for m in re.finditer(r'<div class="screen-wrap">', html)
             if 'display' not in html[m.start():m.start()+50]])
        == len(re.findall(r'<div class="screen-label">', html))
    ),
    '画面フローがtd-left内'          : html.index('id="td-right"') > html.index('id="td-left"'),
    '工数テーブルがtd-right内'       : 'F-0' in html[html.index('id="td-right"'):],
    'サマリー強調UI'                 : 'pulse' in html and '▲ 閉じる' in html,
    'サマリーデフォルト閉じ'          : '#summary-body{' in html and 'display:none' in html,
    '成果物一覧非表示'               : 'display:none' in html[html.index('成果物一覧'):html.index('成果物一覧')+100],
    'scriptブロック残骸なし'         : all(re.search(r'function\s+\w+', s) for s in re.findall(r'<script>(.*?)</script>', html, re.DOTALL) if s.strip()),
}
all_ok = all(checks.values())
for k,v in checks.items(): print(f"{'✅' if v else '❌'} {k}")
print("✅ 全チェック通過" if all_ok else "❌ 要修正")
```

---

## PPTXピッチデッキのルール

**詳細は `skills/wws-pitch-deck/SKILL.md` を参照。**

- レイアウト：A4横 `LAYOUT_WIDE`（13.33" × 7.5"）
- フォント：`fontFace: 'Meiryo'`（日本語文字化け防止）
- 画面フロースライド：HTMLをwkhtmltoimageでラスタライズして貼り込む
- 生成後は必ずLibreOffice→pdftoppmでサムネイル確認

### 画面ラスタライズ手順
```bash
# 各画面のphone-innerを閉じタグ深さカウントで正確に抽出すること
# wkhtmltoimage --width 560 --height {h} --disable-javascript {html} {png}
# → 280pxにリサイズ → phoneフレーム合成（Pillow）
```

---

## WWS社内確認待ち事項

1. CardのスポットにStampSessionペア属性・セッション管理が標準対応しているか（B-01）
2. ReceicoにWebhookが存在するか（B-02の実装方式の分岐点）
3. 両プロダクトのLINEログインチャネルが共通か（B-03の実装方式の分岐点）
4. CardマイページのHTMLカスタマイズ可能範囲（F-04/F-05に影響）
5. 停留所データのGTFSインポート対応有無（B-01に影響）
6. セッション管理がCard標準対応しているか（B-01に影響）

---

## 次のアクション候補

- GTFSデータをアップロードして停留所CSVに変換
- mockup_v5を更新（乗車地検索フロー画面を統合）
- ピッチデッキをクライアントフィードバック後に改版
- wws-proposalスキルを新案件に適用
