# Phase 1 承認記録台帳（2026-07-18）

同期計画（plans/charm-combo-codex-claude-sync-plan.md）Phase 1 のユーザー承認を安定 approvalEvidenceId で記録する。
本台帳が charm-approved-source-freeze.json / charm-approved-color-freeze.json の approvalEvidenceId / approvalEvidenceText の正本となる。
（会話上の承認を要約したもの。実ファイルの同定は SHA-256 による）

## 承認一覧

| approvalEvidenceId | 承認内容（要約） |
|---|---|
| AE-20260718-01 | **pipelineSource = フチ画像×8・全種 sourceMode=colorless / toneMode=flat**。ユーザーは「革の色をきれいに反映させられる方」を選ぶ技術判断を委任 → 背景透過8枚は全て革面までα=0の装飾オーバーレイと実測判明（不透過率0.7〜5.8%）のため、リカラー対象（革面）が存在するフチ画像のみが生成入力になり得ると判断・確定 |
| AE-20260718-02 | **approvalReference = フチ画像兼用**。ユーザー回答「1枚目の画像は、チャームの形として一番見本にしてほしい画像です」（=各チャームのフチ画像）。source と reference は同一ファイル・台帳上は役割分離 |
| AE-20260718-03 | **背景透過8枚はマスク生成の補助資料**として活用（生成入力には使用禁止）。ユーザー説明「背景透過の画像は、革の色をそのまま反映しやすいかと思い作ったもの」— 装飾位置がαで機械的に得られるため manual-owner / manual-keep の下絵に使用する |
| AE-20260718-04 | **canonicalLabel =「シエルブルー」**（公式カラーチャート表記）。現行 COLORS の「シェルブルー」は alias として台帳保持・UI 表示名も変更 |
| AE-20260718-05 | **シェーブルの grainPolicy = distinct（専用 grain を新規作成）**。ニューカラー（シェーブル）チャートから ROI 切り出し（jpeg-roi-luma-straight-v1）。比較シートのユーザー承認後に確定 |
| AE-20260718-06 | **material 表示名: 既存24色 = materialKey `calf`・表示名「カーフ」／新3色 = materialKey `chevre`・表示名「シェーブル」**。UI は革種→色の2段選択（表示条件は AE-20260718-10 を参照） |
| AE-20260718-07 | **swan のロゴ「&.」= あり**（右下・金具鋲の左のシルバー立体表現。ユーザー指摘により確認）。**立体表現のまま採用** — ロゴ所有（G）として metalTone で金/銀再着色し、立体陰影の見た目は Phase 5（形状 QA）で最終判断。ロゴなし時の UI 条件改訂（同期計画の停止ゲート）は不要 |
| AE-20260718-08 | **swan のくちばし = アクセント色として保持（masks.α<128）**。素材に着色して再書き出しする方式を採用（「アクセント色で保持」→「素材に着色して再書き出し（推奨）」の2段階承認） |
| AE-20260718-09 | **swan の pipelineSource =「swanフチ画像　嘴カラーあり.png」**（1701×2560・RGB・αなし・SHA `77c8a315d37a0dad23eb3dd564bdd2baf7a26fe2f0583873de1ff2297461c435`・2026-07-18 20:50 受領）。検証: 四隅白254単色（チェッカーなし）・くちばしブラウン系 17,051px（平均RGB 149,87,44・革シボ付き）を機械確認。**supersededSources（生成入力禁止）= swan.jpg／swanフチ画像.png（チェッカー柄・SHA 847e75bb…）／swanフチ画像２.png（くちばし無着色・SHA 87ad635c…）／swan背景透過.png（装飾オーバーレイ）** |
| AE-20260718-10 | **シェーブル3色（ペールグレージュ／グリーン／ラベンダー）は現行8チャームでは選択不可**。ユーザー指示「現時点で新色のシェーブルの3カラーについては、使えるチャームが限られていて、今お渡ししているチャームでは使えません。…一旦こちらのチャーム選択画面の時は、新しい3色のカラーは選択できないような仕様にしてほしい」。実装後に追加されるアイテムで選択可能になる予定。→ **27 leatherVariant の定義（hex・専用 grain・material profile）は将来用の正本として凍結しつつ、シェーブル3 variant の appliesToCharms = 空**。数量契約は `finiteCases = Σ variants.appliesToCharms.length = 24色×8チャーム = 192`（216 を置換）。クライアント色確認も 192 組。UI は「選択可能な material が2種以上ある場合のみ革種選択を表示」とし、現段階の charms.html はカーフ24色のみの表示となる |

## 数量契約への影響（AE-20260718-10 による確定値）

- leatherVariants = 27（定義数・color freeze 対象は不変）
- materialProfiles = 2（calf / chevre — chevre は将来アイテム用の正本）
- **finiteCases = 192**（8チャーム × 有効24 variant。216 から置換 — 同期計画 §5-1 の 8 の規定どおり再計算）
- クライアント色確認（Phase 7）= 192 entry
- charms.html の variant 選択肢 = calf 24 のみ（chevre 3 は appliesToCharms=空により非表示）

## 実測記録（機械検証）

- 候補台帳突合: フチ画像8点・背景透過8点・チャート5点の寸法・SHA-256 が同期計画 §3／§3-1 の台帳と完全一致（source-candidates.json / color-candidates.json 参照）
- フチ画像の背景: 7種＋swan嘴カラーあり版はコーナー100×100が輝度254単色。旧 swanフチ画像.png のみチェッカー柄（79色種・輝度242..254）で不適 → 差し替え済み
- チャートのパネル構成: レッド系・イエロー系・ブラウン系・ブルー系 = 2列×3行の6パネル／ニューカラー（シェーブル）= 上段全幅1（ペールグレージュ）＋下段2（グリーン・ラベンダー）の3パネル。計27
