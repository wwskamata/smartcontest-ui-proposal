# WWS提案プロジェクト｜Claude Code 設定ファイル

このファイルはClaude Codeがプロジェクトフォルダを開いた際に自動で読み込まれます。

---

## プロジェクト概要

このフォルダは複数案件・プロダクトの提案資料を管理する共通プロジェクトフォルダです。

### 案件①：公共交通利用促進キャンペーン
**依頼者**：ビザビ社（代理店）→ 行政クライアントへ提案  
**プロダクト**：SmartStampCard × SmartRecieco（プランA）/ + MapPenguin（プランB）
**プランB仕様**：乗車1回=1pt・GTFSなし・ペア判定なし・MapPenguinでバス停地図検索・2キャンペーン構成

### 案件②：店舗物件検索プラットフォーム開発
**依頼者**：株式会社アントレサポート（東京都・起業支援）  
**プロダクト**：Admin-DX CMS（Kuroco）× Next.js  
**ステータス**：予算・企画確定済み → サービス選定フェーズ（2026-05時点）

---

## フォルダ構成

```
wws-local-project/
├── CLAUDE.md                        ← このファイル（常に参照）
├── assets/                          ← 案件成果物
│   ├── mockup_v5_transit_campaign.html         ← [案件①-A] 社内総合シート プランA（確定版）
│   ├── mockup_planb_transit_campaign.html      ← [案件①-B] 社内総合シート プランB（MapPenguin連携・12画面）
│   ├── mockup_stamp_flow.html                  ← [案件①] 乗車地検索フロー 全13画面
│   ├── project_summary_transit_campaign.html   ← [案件①] 案件サマリー
│   ├── project_summary_transit_campaign.md     ← [案件①] 案件サマリー（Markdown）
│   ├── custom_requirements_for_SE.md           ← [案件①] SE向け機能要件定義書
│   └── project_summary_admin_dx_entre_support.md  ← [案件②] アントレサポート案件サマリー
├── generate_pitch_entre_support.js  ← [案件②] PPTX生成スクリプト（node で実行）
├── generated/                       ← 生成済みPPTX
│   ├── pitch_entre_support_202605.pptx  ← [案件②] アントレサポート提案デッキ ✅
│   ├── pitch_transit_v3_202505.pptx     ← [案件①] 最新提案デッキ（推奨）
│   └── pitch_transit_v2_202505.pptx     ← [案件①] 旧版
├── .mcp.json                        ← MCP サーバー設定（wws-skills 登録済み）
└── skills/                          ← スキルファイル
    ├── wws-ws-saas-spec-html-internal/  ← 社内総合シート・仕様書生成スキル（旧wws-proposal）
    │   ├── SKILL.md
    │   └── references/
    │       ├── mockup-layout.md      ← レイアウト確定ルール（必読）
    │       ├── products.md           ← プロダクト仕様・料金
    │       └── se-requirements.md   ← 工数算定テンプレ・スキル名定義
    ├── wws-ws-saas-hearing-pptx-public/ ← 提案先向けPPTX生成スキル（旧wws-pitch-deck）
    │   ├── SKILL.md
    │   └── references/
    │       └── slide-templates.md   ← 各スライドのpptxgenjsコード
    ├── wws-publish/                 ← HTML暗号化 + GitHub Pages 公開スキル
    │   └── SKILL.md                 ← staticrypt + gh CLI の手順（必読）
    └── admin-dx-cms/               ← Admin-DX CMS（Kuroco）提案スキル
        ├── SKILL.md
        └── references/
            └── products.md          ← kuroco製品知識・競合比較・注意事項
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

## WWS社内確認待ち事項

1. CardのスポットにStampSessionペア属性・セッション管理が標準対応しているか（B-01）
2. ReceicoにWebhookが存在するか（B-02の実装方式の分岐点）
3. 両プロダクトのLINEログインチャネルが共通か（B-03の実装方式の分岐点）
4. CardマイページのHTMLカスタマイズ可能範囲（F-04/F-05に影響）
5. 停留所データのGTFSインポート対応有無（B-01に影響）
6. セッション管理がCard標準対応しているか（B-01に影響）

---

## 次のアクション候補

- **[完了]** プランB PPTX生成（pitch_transit_planb_202505.pptx）
- **[完了]** プランB HTMLモックアップ（mockup_planb_transit_campaign.html）
- **[完了]** wws-publish スキル作成（skills/wws-publish/SKILL.md）
- **[完了]** mockup_planb を staticrypt で暗号化してGitHub Pagesに公開
  → URL: https://wwskamata.github.io/transit-planb/ / PW: Transit2026
- **[完了]** Claude.ai プロジェクト用ファイル生成（claude_ai_project/ 以下）
- **[完了]** MCP サーバー構築・登録（C:\Users\kamat\wws-mcp-server\server.js）
  → create_html_mockup / create_pptx / publish_html / list_projects の4ツール利用可能
  → .mcp.json（プロジェクト）＋ ~/.claude.json（グローバル）の両方に登録済み
- ピッチデッキをクライアントフィードバック後に改版
- wws-ws-saas-spec-html-internalスキルを新案件に適用
