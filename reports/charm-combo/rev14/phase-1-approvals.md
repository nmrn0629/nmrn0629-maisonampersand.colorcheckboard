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
| AE-20260718-11 | **カーフ24色の approvedHex = チャート抽出値（B）を全色採用**。比較シート（reports/charm-combo/rev14/color-sheet/index.html — A=現行UI値 vs B=チャート抽出値・A/B間の最大差はピスタチオ ΔE 2.9）を提示し、ユーザー回答「そのおすすめの方で進めてもらって大丈夫です」（おすすめ=カーフ24色は全部B・シェーブルはB2）。samplingMethod=`roi-lab-lower-median-v1`（recomputedProposedHex=proposedHex=approvedHex）。抽出条件: パネル境界検出=`lab-adjacent-diff-transition-band-v1`（予想境界±8%窓内で隣接行/列平均の Lab ΔE>1.8 の遷移帯を除外。白線前提の初版検出はブラウン系/ブルー系で誤検出したため置換）・samplingRect=パネル上12%〜58%の帯・左右8%マージン（下部の色名文字を回避）・exclusionMask=全0・全27パネル均一性検査PASS（samplingRect内の隣接行/列平均ΔE≦1.8・レンジΔE≦25）。panelRect/samplingRect/Lab下位中央値/decodedRgbaSha256 の正本=chart-panels.json（本ディレクトリ） |
| AE-20260718-12 | **シェーブル3色の approvedHex = P3→sRGB変換値（B2）を採用**（同上のユーザー回答）。チャート「ニューカラー（シェーブル）.jpg」のみ **Display P3 Gamut with sRGB Transfer** プロファイル（他4枚はsRGB）のため、生数値のstraight解釈（B1）ではなく実際の見え方に忠実なP3→sRGB変換値を選択。変換=sRGB transferでリニア化→P3→XYZ(D65)行列→Lab（下位中央値はP3解釈Labで採取）→sRGB戻し。**グリーンはsRGB色域外→L/h固定・C二分探索20回で彩度圧縮**（改訂13 flatLeatherTone と同一規則）。freeze時の記録方式: `roi-lab-lower-median-v1` の独立再計算（recomputedProposedHex）はstraight解釈のみを一致対象とするため、シェーブル3色は **samplingMethod=`approved-explicit-hex`**（本Evidence IDと変換手順を記録）とし、straight値（B1: #9c8e89/#31894d/#8289dd）はchart-panels.jsonに参考保存 |
| AE-20260718-13 | **シェーブル専用grainの正本ソース = ユーザー提供のベージュ質感アップ写真（候補2）**。ユーザーがチャット添付でシェーブル質感画像4枚を提供（2026-07-18）し、比較シートv2（grain-sheet/index.html — カーフ+3候補を同一条件で luma 化・同一色タイル適用・blur12px/±0.35）に対し「候補2にしよう」と承認。これにより AE-20260718-05 の distinct 方針が最終確定。**正本**: 原本 1000090060.jpg → Git管理名「**シェーブル質感.jpg**」（素材ルート直下・844×403・JPEG・Display P3 Gamut with sRGB Transfer・SHA `f0a65c5b841285a1e34f9f39a7d9500ad414a23c11569c5b94c7a42b0492518d`）。grain抽出は jpeg-roi-luma-straight-v1（輝度のみ）のためプロファイルの影響なし。**採用ROI = 全面から縁10px除外 {x:10, y:10, w:824, h:383}**。**参考資料（生成入力禁止）**: 候補1=シェーブル質感ブルー.jpg（522×377・SHA `8557ba58…`）／候補3元画像=シェーブル製品写真グリーン.jpg（1079×767・SHA `0358b73c…`・検証済み代替ROI x230,y545,w620,h130）／シェーブル製品写真ブルーグレージュ.jpg（1079×893・SHA `94aa9736…`・将来アイテムの形状参考）。**フォールバック（不使用）**: ニューカラーチャートのペールグレージュ領域ROI（x24,y24,w1032,h425・シートv1で方向性のみ確認済みだった旧案） |

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

## 確定 approvedHex 一覧（AE-20260718-11／AE-20260718-12・2026-07-18 承認）

数値の正本は chart-panels.json（labLowerMedian・panelRect・samplingRect・decodedRgbaSha256 込み）。本表は人間可読の転記で、freeze JSON 作成時に chart-panels.json から機械再取込して本表と照合する。

| chart | panelIndex | canonicalLabel | approvedHex | 由来 |
|---|---|---|---|---|
| red | 0 | ピンク | #f78192 | B（チャート抽出値） |
| red | 1 | レッド | #e45c61 | B（チャート抽出値） |
| red | 2 | ローズピンク | #a93f74 | B（チャート抽出値） |
| red | 3 | パープル | #a855a2 | B（チャート抽出値） |
| red | 4 | ペールピンク | #e7cbda | B（チャート抽出値） |
| red | 5 | コーラルピンク | #ef9f7e | B（チャート抽出値） |
| yellow | 0 | ゴールド | #c8925f | B（チャート抽出値） |
| yellow | 1 | オレンジ | #e59134 | B（チャート抽出値） |
| yellow | 2 | ライムイエロー | #f0e15c | B（チャート抽出値） |
| yellow | 3 | ペールイエロー | #f4ebb8 | B（チャート抽出値） |
| yellow | 4 | ブリック | #885854 | B（チャート抽出値） |
| yellow | 5 | ピスタチオ | #94ad79 | B（チャート抽出値） |
| brown | 0 | ブラック | #41403d | B（チャート抽出値） |
| brown | 1 | グレージュ | #82756b | B（チャート抽出値） |
| brown | 2 | ブラウン | #935a45 | B（チャート抽出値） |
| brown | 3 | チャコールグレー | #616062 | B（チャート抽出値） |
| brown | 4 | アイボリー | #ece7dd | B（チャート抽出値） |
| brown | 5 | エトープ | #a48e73 | B（チャート抽出値） |
| blue | 0 | ライトブルー | #b0ece8 | B（チャート抽出値） |
| blue | 1 | シエルブルー | #6cbacf | B（チャート抽出値） |
| blue | 2 | ミストブルー | #7aaabf | B（チャート抽出値） |
| blue | 3 | アイスグレー | #cbcdcc | B（チャート抽出値） |
| blue | 4 | ロイヤルブルー | #445fc1 | B（チャート抽出値） |
| blue | 5 | ネイビー | #3c4d61 | B（チャート抽出値） |
| chevre | 0 | ペールグレージュ | #9e8e89 | B2（P3→sRGB変換） |
| chevre | 1 | グリーン | #00894c | B2（P3→sRGB変換）・sRGB色域外→C圧縮 |
| chevre | 2 | ラベンダー | #8089e3 | B2（P3→sRGB変換） |

補足: アイボリーの approvedHex はチャート由来の #ece7dd だが、チャーム表示上の DISPLAY_COLORS では改訂13 §5-6 のとおり referenceIvory（frenchie実物色基準）へ差し替えられる（COLORS 正本値とUI表示の二層構造は変更なし）。
